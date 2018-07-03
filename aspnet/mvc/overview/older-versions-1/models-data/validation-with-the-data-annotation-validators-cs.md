---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
title: Convalida con i validator di annotazione dei dati (c#) | Microsoft Docs
author: microsoft
description: È possibile sfruttare il raccoglitore di modelli di annotazione dei dati per eseguire la convalida all'interno di un'applicazione ASP.NET MVC. Informazioni su come usare i diversi tipi di convalida...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 7ca8013e-9dfc-4e33-8336-cdccfd5f9414
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
msc.type: authoredcontent
ms.openlocfilehash: 484d836c3865ca3df684a379585b71138d42ab7a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378217"
---
<a name="validation-with-the-data-annotation-validators-c"></a><span data-ttu-id="34d5f-104">Convalida con i validator di annotazione dei dati (c#)</span><span class="sxs-lookup"><span data-stu-id="34d5f-104">Validation with the Data Annotation Validators (C#)</span></span>
====================
<span data-ttu-id="34d5f-105">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="34d5f-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="34d5f-106">È possibile sfruttare il raccoglitore di modelli di annotazione dei dati per eseguire la convalida all'interno di un'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="34d5f-106">Take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="34d5f-107">Informazioni su come usare i diversi tipi di attributi di convalida e usarle in Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="34d5f-107">Learn how to use the different types of validator attributes and work with them in the Microsoft Entity Framework.</span></span>


<span data-ttu-id="34d5f-108">In questa esercitazione descrive come usare i validator di annotazione dei dati per eseguire la convalida in un'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="34d5f-108">In this tutorial, you learn how to use the Data Annotation validators to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="34d5f-109">Il vantaggio di usare i validator di annotazione dei dati è che consentono di eseguire la convalida mediante la semplice aggiunta di uno o più attributi, come richiesto o un attributo StringLength: per una proprietà di classe.</span><span class="sxs-lookup"><span data-stu-id="34d5f-109">The advantage of using the Data Annotation validators is that they enable you to perform validation simply by adding one or more attributes – such as the Required or StringLength attribute – to a class property.</span></span>

<span data-ttu-id="34d5f-110">Prima di poter usare i validator di annotazione dei dati, è necessario scaricare lo strumento individuerebbe annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="34d5f-110">Before you can use the Data Annotation validators, you must download the Data Annotations Model Binder.</span></span> <span data-ttu-id="34d5f-111">È possibile scaricare l'esempio di strumento di associazione di modello annotazioni dati dal sito Web CodePlex facendo [qui](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span><span class="sxs-lookup"><span data-stu-id="34d5f-111">You can download the Data Annotations Model Binder Sample from the CodePlex website by clicking [here](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span></span>


<span data-ttu-id="34d5f-112">È importante comprendere che lo strumento individuerebbe annotazioni dei dati non è una parte ufficiale di Microsoft ASP.NET MVC framework.</span><span class="sxs-lookup"><span data-stu-id="34d5f-112">It is important to understand that the Data Annotations Model Binder is not an official part of the Microsoft ASP.NET MVC framework.</span></span> <span data-ttu-id="34d5f-113">Anche se lo strumento individuerebbe annotazioni dei dati è stato creato dal team di Microsoft ASP.NET MVC, Microsoft non offre il supporto ufficiale del prodotto per lo strumento individuerebbe annotazioni dei dati descritti e usati in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="34d5f-113">Although the Data Annotations Model Binder was created by the Microsoft ASP.NET MVC team, Microsoft does not offer official product support for the Data Annotations Model Binder described and used in this tutorial.</span></span>


## <a name="using-the-data-annotation-model-binder"></a><span data-ttu-id="34d5f-114">Usando lo strumento individuerebbe annotazione dei dati</span><span class="sxs-lookup"><span data-stu-id="34d5f-114">Using the Data Annotation Model Binder</span></span>

<span data-ttu-id="34d5f-115">Per usare lo strumento individuerebbe annotazioni dei dati in un'applicazione ASP.NET MVC, è necessario aggiungere un riferimento all'assembly Microsoft.Web.Mvc.DataAnnotations.dll e DataAnnotations assembly.</span><span class="sxs-lookup"><span data-stu-id="34d5f-115">In order to use the Data Annotations Model Binder in an ASP.NET MVC application, you first need to add a reference to the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly.</span></span> <span data-ttu-id="34d5f-116">Selezionare l'opzione di menu **progetto, Aggiungi riferimento**.</span><span class="sxs-lookup"><span data-stu-id="34d5f-116">Select the menu option **Project, Add Reference**.</span></span> <span data-ttu-id="34d5f-117">Successivamente fare clic sui **esplorare** scheda e selezionare il percorso in cui è stato scaricato (e decompresso) l'esempio di Raccoglitore di modelli di annotazioni dei dati (vedere **figura 1**).</span><span class="sxs-lookup"><span data-stu-id="34d5f-117">Next click the **Browse** tab and browse to the location where you downloaded (and unzipped) the Data Annotations Model Binder sample (see **Figure 1**).</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image2.png)](validation-with-the-data-annotation-validators-cs/_static/image1.png)

<span data-ttu-id="34d5f-118">**Figura 1**: aggiunta di un riferimento al Binder di modello di annotazioni dei dati ([fare clic per visualizzare l'immagine con dimensioni normali](validation-with-the-data-annotation-validators-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="34d5f-118">**Figure 1**: Adding a reference to the Data Annotations Model Binder ([Click to view full-size image](validation-with-the-data-annotation-validators-cs/_static/image3.png))</span></span>

<span data-ttu-id="34d5f-119">Selezionare sia la Microsoft.Web.Mvc.DataAnnotations.dll agli assembly e DataAnnotations e scegliere il **OK** pulsante.</span><span class="sxs-lookup"><span data-stu-id="34d5f-119">Select both the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly and click the **OK** button.</span></span>


<span data-ttu-id="34d5f-120">È possibile usare l'assembly DataAnnotations incluso in .NET Framework Service Pack 1 con lo strumento individuerebbe annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="34d5f-120">You cannot use the System.ComponentModel.DataAnnotations.dll assembly included with .NET Framework Service Pack 1 with the Data Annotations Model Binder.</span></span> <span data-ttu-id="34d5f-121">È necessario usare la versione dell'assembly DataAnnotations incluso con il download di esempio dello strumento di associazione di dati le annotazioni del modello.</span><span class="sxs-lookup"><span data-stu-id="34d5f-121">You must use the version of the System.ComponentModel.DataAnnotations.dll assembly included with the Data Annotations Model Binder Sample download.</span></span>


<span data-ttu-id="34d5f-122">Infine, è necessario registrare il gestore di associazione di modelli di DataAnnotations nel file Global. asax.</span><span class="sxs-lookup"><span data-stu-id="34d5f-122">Finally, you need to register the DataAnnotations Model Binder in the Global.asax file.</span></span> <span data-ttu-id="34d5f-123">Aggiungere la riga di codice seguente all'applicazione\_gestore dell'evento Start () in modo che l'applicazione\_metodo Start () si presenta come segue:</span><span class="sxs-lookup"><span data-stu-id="34d5f-123">Add the following line of code to the Application\_Start() event handler so that the Application\_Start() method looks like this:</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample1.cs)]

<span data-ttu-id="34d5f-124">Questa riga di codice registra l'ataAnnotationsModelBinder come il raccoglitore dei modelli predefiniti per l'intera applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="34d5f-124">This line of code registers the ataAnnotationsModelBinder as the default model binder for the entire ASP.NET MVC application.</span></span>

## <a name="using-the-data-annotation-validator-attributes"></a><span data-ttu-id="34d5f-125">Usando gli attributi di Validator di annotazione dei dati</span><span class="sxs-lookup"><span data-stu-id="34d5f-125">Using the Data Annotation Validator Attributes</span></span>

<span data-ttu-id="34d5f-126">Quando si usa lo strumento individuerebbe annotazioni dei dati, si usano attributi di convalida per eseguire la convalida.</span><span class="sxs-lookup"><span data-stu-id="34d5f-126">When you use the Data Annotations Model Binder, you use validator attributes to perform validation.</span></span> <span data-ttu-id="34d5f-127">Lo spazio dei nomi System.ComponentModel.DataAnnotations include gli attributi di convalida seguenti:</span><span class="sxs-lookup"><span data-stu-id="34d5f-127">The System.ComponentModel.DataAnnotations namespace includes the following validator attributes:</span></span>

- <span data-ttu-id="34d5f-128">Intervallo: consente di verificare se il valore di una proprietà è compreso tra un determinato intervallo di valori.</span><span class="sxs-lookup"><span data-stu-id="34d5f-128">Range – Enables you to validate whether the value of a property falls between a specified range of values.</span></span>
- <span data-ttu-id="34d5f-129">RegularExpression: consente di verificare se il valore di una proprietà corrisponde a un modello di espressione regolare specificata.</span><span class="sxs-lookup"><span data-stu-id="34d5f-129">RegularExpression – Enables you to validate whether the value of a property matches a specified regular expression pattern.</span></span>
- <span data-ttu-id="34d5f-130">È necessario: consente di contrassegnare una proprietà in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="34d5f-130">Required – Enables you to mark a property as required.</span></span>
- <span data-ttu-id="34d5f-131">StringLength – consente di specificare una lunghezza massima per una proprietà di stringa.</span><span class="sxs-lookup"><span data-stu-id="34d5f-131">StringLength – Enables you to specify a maximum length for a string property.</span></span>
- <span data-ttu-id="34d5f-132">Convalida-classe di base per tutti gli attributi di convalida.</span><span class="sxs-lookup"><span data-stu-id="34d5f-132">Validation – The base class for all validator attributes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="34d5f-133">Se le esigenze di convalida non sono soddisfatte da una delle funzioni di convalida standard quindi avere sempre la possibilità di creare un attributo di validator personalizzato ereditando un nuovo attributo di convalida dall'attributo di convalida di base.</span><span class="sxs-lookup"><span data-stu-id="34d5f-133">If your validation needs are not satisfied by any of the standard validators then you always have the option of creating a custom validator attribute by inheriting a new validator attribute from the base Validation attribute.</span></span>


<span data-ttu-id="34d5f-134">La classe di prodotto nel **listato 1** illustra come usare questi attributi di convalida.</span><span class="sxs-lookup"><span data-stu-id="34d5f-134">The Product class in **Listing 1** illustrates how to use these validator attributes.</span></span> <span data-ttu-id="34d5f-135">La proprietà Name, Description e UnitPrice sono contrassegnate come richiesto.</span><span class="sxs-lookup"><span data-stu-id="34d5f-135">The Name, Description, and UnitPrice properties are marked as required.</span></span> <span data-ttu-id="34d5f-136">La proprietà del nome deve avere una lunghezza di stringa contenente meno di 10 caratteri.</span><span class="sxs-lookup"><span data-stu-id="34d5f-136">The Name property must have a string length that is less than 10 characters.</span></span> <span data-ttu-id="34d5f-137">Infine, la proprietà UnitPrice deve corrispondere un criterio di espressione regolare che rappresenta un importo di valuta.</span><span class="sxs-lookup"><span data-stu-id="34d5f-137">Finally, the UnitPrice property must match a regular expression pattern that represents a currency amount.</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample2.cs)]

<span data-ttu-id="34d5f-138">**Listato 1**: Models\Product.cs</span><span class="sxs-lookup"><span data-stu-id="34d5f-138">**Listing 1**: Models\Product.cs</span></span>

<span data-ttu-id="34d5f-139">La classe di prodotto viene illustrato come utilizzare un attributo aggiuntivo: l'attributo DisplayName.</span><span class="sxs-lookup"><span data-stu-id="34d5f-139">The Product class illustrates how to use one additional attribute: the DisplayName attribute.</span></span> <span data-ttu-id="34d5f-140">L'attributo DisplayName consente di modificare il nome della proprietà quando la proprietà viene visualizzata in un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="34d5f-140">The DisplayName attribute enables you to modify the name of the property when the property is displayed in an error message.</span></span> <span data-ttu-id="34d5f-141">Invece di visualizzare il messaggio di errore "il campo UnitPrice è obbligatorio" è possibile visualizzare il messaggio di errore "il campo del prezzo è obbligatorio".</span><span class="sxs-lookup"><span data-stu-id="34d5f-141">Instead of displaying the error message "The UnitPrice field is required" you can display the error message "The Price field is required".</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="34d5f-142">Se si desidera personalizzare completamente il messaggio di errore visualizzato da un servizio validator è possibile assegnare un messaggio di errore personalizzato per la proprietà del validator messaggio di errore simile al seguente: `<Required(ErrorMessage:="This field needs a value!")>`</span><span class="sxs-lookup"><span data-stu-id="34d5f-142">If you want to completely customize the error message displayed by a validator then you can assign a custom error message to the validator's ErrorMessage property like this: `<Required(ErrorMessage:="This field needs a value!")>`</span></span>


<span data-ttu-id="34d5f-143">È possibile usare la classe di prodotto nel **listato 1** con l'azione del controller create () in **listato 2**.</span><span class="sxs-lookup"><span data-stu-id="34d5f-143">You can use the Product class in **Listing 1** with the Create() controller action in **Listing 2**.</span></span> <span data-ttu-id="34d5f-144">Questa azione del controller Visualizza di nuovo la visualizzazione Create quando lo stato del modello contiene eventuali errori.</span><span class="sxs-lookup"><span data-stu-id="34d5f-144">This controller action redisplays the Create view when model state contains any errors.</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample3.cs)]

<span data-ttu-id="34d5f-145">**Listato 2**: Controllers\ProductController.vb</span><span class="sxs-lookup"><span data-stu-id="34d5f-145">**Listing 2**: Controllers\ProductController.vb</span></span>

<span data-ttu-id="34d5f-146">Infine, è possibile creare la vista **listato 3** facendo clic su azione create () e selezionando l'opzione di menu **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="34d5f-146">Finally, you can create the view in **Listing 3** by right-clicking the Create() action and selecting the menu option **Add View**.</span></span> <span data-ttu-id="34d5f-147">Creare una visualizzazione fortemente tipizzata con la classe Product come la classe del modello.</span><span class="sxs-lookup"><span data-stu-id="34d5f-147">Create a strongly-typed view with the Product class as the model class.</span></span> <span data-ttu-id="34d5f-148">Selezionare **Create** nell'elenco a discesa contenuto visualizzazione (vedere **Figura2**).</span><span class="sxs-lookup"><span data-stu-id="34d5f-148">Select **Create** from the view content dropdown list (see **Figure 2**).</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image5.png)](validation-with-the-data-annotation-validators-cs/_static/image4.png)

<span data-ttu-id="34d5f-149">**Figura 2**: aggiunta della visualizzazione Create</span><span class="sxs-lookup"><span data-stu-id="34d5f-149">**Figure 2**: Adding the Create View</span></span>

[!code-aspx[Main](validation-with-the-data-annotation-validators-cs/samples/sample4.aspx)]

<span data-ttu-id="34d5f-150">**Listato 3**: Views\Product\Create.aspx</span><span class="sxs-lookup"><span data-stu-id="34d5f-150">**Listing 3**: Views\Product\Create.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="34d5f-151">Rimuovere il campo Id dal form crea generato per il **Aggiungi visualizzazione** l'opzione di menu.</span><span class="sxs-lookup"><span data-stu-id="34d5f-151">Remove the Id field from the Create form generated by the **Add View** menu option.</span></span> <span data-ttu-id="34d5f-152">Poiché il campo Id corrisponde a una colonna Identity, non si vuole consentire agli utenti di immettere un valore per questo campo.</span><span class="sxs-lookup"><span data-stu-id="34d5f-152">Because the Id field corresponds to an Identity column, you don't want to allow users to enter a value for this field.</span></span>


<span data-ttu-id="34d5f-153">Se si invia il modulo per la creazione di un prodotto e non si immette i valori per i campi obbligatori, quindi i messaggi di errore di convalida nella **figura 3** vengono visualizzati.</span><span class="sxs-lookup"><span data-stu-id="34d5f-153">If you submit the form for creating a Product and you do not enter values for the required fields, then the validation error messages in **Figure 3** are displayed.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image7.png)](validation-with-the-data-annotation-validators-cs/_static/image6.png)

<span data-ttu-id="34d5f-154">**Figura 3**: campi obbligatori mancanti</span><span class="sxs-lookup"><span data-stu-id="34d5f-154">**Figure 3**: Missing required fields</span></span>

<span data-ttu-id="34d5f-155">Se si immette un importo di valuta non è valido, quindi il messaggio di errore in **figura 4** viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="34d5f-155">If you enter an invalid currency amount, then the error message in **Figure 4** is displayed.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image9.png)](validation-with-the-data-annotation-validators-cs/_static/image8.png)

<span data-ttu-id="34d5f-156">**Figura 4**: importo di valuta non è valido</span><span class="sxs-lookup"><span data-stu-id="34d5f-156">**Figure 4**: Invalid currency amount</span></span>

## <a name="using-data-annotation-validators-with-the-entity-framework"></a><span data-ttu-id="34d5f-157">Uso di validator di annotazione dei dati con Entity Framework</span><span class="sxs-lookup"><span data-stu-id="34d5f-157">Using Data Annotation Validators with the Entity Framework</span></span>

<span data-ttu-id="34d5f-158">Se si usa Microsoft Entity Framework per generare classi del modello di dati è possibile applicare gli attributi di convalida direttamente alle classi.</span><span class="sxs-lookup"><span data-stu-id="34d5f-158">If you are using the Microsoft Entity Framework to generate your data model classes then you cannot apply the validator attributes directly to your classes.</span></span> <span data-ttu-id="34d5f-159">Poiché la finestra di progettazione di Entity Framework genera le classi del modello, le modifiche apportate alle classi di modello verranno sovrascritto alla successiva che si apportano modifiche nella finestra di progettazione.</span><span class="sxs-lookup"><span data-stu-id="34d5f-159">Because the Entity Framework Designer generates the model classes, any changes you make to the model classes will be overwritten the next time you make any changes in the Designer.</span></span>

<span data-ttu-id="34d5f-160">Se si desidera usare i validator con le classi generate da Entity Framework è necessario creare classi di dati di metadati.</span><span class="sxs-lookup"><span data-stu-id="34d5f-160">If you want to use the validators with the classes generated by the Entity Framework then you need to create meta data classes.</span></span> <span data-ttu-id="34d5f-161">I validator è applicare alla classe di dati di metadati anziché applicare i validator alla classe effettiva.</span><span class="sxs-lookup"><span data-stu-id="34d5f-161">You apply the validators to the meta data class instead of applying the validators to the actual class.</span></span>

<span data-ttu-id="34d5f-162">Ad esempio, immaginare che hanno creato una classe di film mediante Entity Framework (vedere **figura 5**).</span><span class="sxs-lookup"><span data-stu-id="34d5f-162">For example, imagine that you have created a Movie class using the Entity Framework (see **Figure 5**).</span></span> <span data-ttu-id="34d5f-163">Si supponga, inoltre, che si desidera rendere il titolo del film e direttore proprietà necessarie di proprietà.</span><span class="sxs-lookup"><span data-stu-id="34d5f-163">Imagine, furthermore, that you want to make the Movie Title and Director properties required properties.</span></span> <span data-ttu-id="34d5f-164">In tal caso, è possibile creare la classe parziale e metaclasse dati in **listato 4**.</span><span class="sxs-lookup"><span data-stu-id="34d5f-164">In that case, you can create the partial class and meta data class in **Listing 4**.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image11.png)](validation-with-the-data-annotation-validators-cs/_static/image10.png)

<span data-ttu-id="34d5f-165">**Figura 5**: classe film generato da Entity Framework</span><span class="sxs-lookup"><span data-stu-id="34d5f-165">**Figure 5**: Movie class generated by Entity Framework</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample5.cs)]

<span data-ttu-id="34d5f-166">**Listato 4**: Models\Movie.cs</span><span class="sxs-lookup"><span data-stu-id="34d5f-166">**Listing 4**: Models\Movie.cs</span></span>

<span data-ttu-id="34d5f-167">Il file nella **listato 4** contiene due classi denominate film e MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="34d5f-167">The file in **Listing 4** contains two classes named Movie and MovieMetaData.</span></span> <span data-ttu-id="34d5f-168">La classe di film è una classe parziale.</span><span class="sxs-lookup"><span data-stu-id="34d5f-168">The Movie class is a partial class.</span></span> <span data-ttu-id="34d5f-169">Corrisponde alla classe parziale generata da Entity Framework che è contenuto nel file DataModel.Designer.vb.</span><span class="sxs-lookup"><span data-stu-id="34d5f-169">It corresponds to the partial class generated by the Entity Framework that is contained in the DataModel.Designer.vb file.</span></span>

<span data-ttu-id="34d5f-170">Attualmente, .NET framework non supporta proprietà parziale.</span><span class="sxs-lookup"><span data-stu-id="34d5f-170">Currently, the .NET framework does not support partial properties.</span></span> <span data-ttu-id="34d5f-171">Pertanto, non è possibile applicare gli attributi di convalida per le proprietà della classe film definito nel file DataModel.Designer.vb applicando gli attributi di convalida per le proprietà della classe film definito nel file **listato 4**.</span><span class="sxs-lookup"><span data-stu-id="34d5f-171">Therefore, there is no way to apply the validator attributes to the properties of the Movie class defined in the DataModel.Designer.vb file by applying the validator attributes to the properties of the Movie class defined in the file in **Listing 4**.</span></span>

<span data-ttu-id="34d5f-172">Si noti che la classe parziale del film è decorata con un attributo MetadataType che punta alla classe MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="34d5f-172">Notice that the Movie partial class is decorated with a MetadataType attribute that points at the MovieMetaData class.</span></span> <span data-ttu-id="34d5f-173">La classe MovieMetaData contiene proprietà proxy per le proprietà della classe di film.</span><span class="sxs-lookup"><span data-stu-id="34d5f-173">The MovieMetaData class contains proxy properties for the properties of the Movie class.</span></span>

<span data-ttu-id="34d5f-174">Gli attributi di convalida vengono applicati alle proprietà della classe MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="34d5f-174">The validator attributes are applied to the properties of the MovieMetaData class.</span></span> <span data-ttu-id="34d5f-175">Le proprietà Title, Director e DateReleased tutti vengono contrassegnate come proprietà obbligatorie.</span><span class="sxs-lookup"><span data-stu-id="34d5f-175">The Title, Director, and DateReleased properties are all marked as required properties.</span></span> <span data-ttu-id="34d5f-176">La proprietà Director deve essere assegnata una stringa contenente meno di 5 caratteri.</span><span class="sxs-lookup"><span data-stu-id="34d5f-176">The Director property must be assigned a string that contains less than 5 characters.</span></span> <span data-ttu-id="34d5f-177">Infine, viene applicato l'attributo DisplayName alla proprietà DateReleased per visualizzare un messaggio di errore, ad esempio "il campo rilasciato data è obbligatorio".</span><span class="sxs-lookup"><span data-stu-id="34d5f-177">Finally, the DisplayName attribute is applied to the DateReleased property to display an error message like "The Date Released field is required."</span></span> <span data-ttu-id="34d5f-178">anziché l'errore "il campo DateReleased è obbligatorio".</span><span class="sxs-lookup"><span data-stu-id="34d5f-178">instead of the error "The DateReleased field is required."</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="34d5f-179">Si noti che la proprietà nella classe MovieMetaData proxy non è necessario rappresentare gli stessi tipi di proprietà corrispondenti nella classe di film.</span><span class="sxs-lookup"><span data-stu-id="34d5f-179">Notice that the proxy properties in the MovieMetaData class do not need to represent the same types as the corresponding properties in the Movie class.</span></span> <span data-ttu-id="34d5f-180">Ad esempio, la proprietà Director è una proprietà di stringa della classe di film e proprietà di un oggetto della classe MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="34d5f-180">For example, the Director property is a string property in the Movie class and an object property in the MovieMetaData class.</span></span>


<span data-ttu-id="34d5f-181">La pagina **figura 6** illustra i messaggi di errore restituiti quando si immettono valori non validi per le proprietà di film.</span><span class="sxs-lookup"><span data-stu-id="34d5f-181">The page in **Figure 6** illustrates the error messages returned when you enter invalid values for the Movie properties.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image13.png)](validation-with-the-data-annotation-validators-cs/_static/image12.png)

<span data-ttu-id="34d5f-182">**Figura 6**: tramite validator con Entity Framework ([fare clic per visualizzare l'immagine con dimensioni normali](validation-with-the-data-annotation-validators-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="34d5f-182">**Figure 6**: Using validators with the Entity Framework ([Click to view full-size image](validation-with-the-data-annotation-validators-cs/_static/image14.png))</span></span>

## <a name="summary"></a><span data-ttu-id="34d5f-183">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="34d5f-183">Summary</span></span>

<span data-ttu-id="34d5f-184">In questa esercitazione è stato descritto come sfruttare il raccoglitore di modelli di annotazione dei dati per eseguire la convalida all'interno di un'applicazione ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="34d5f-184">In this tutorial, you learned how to take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="34d5f-185">Si è appreso come usare i diversi tipi di attributi di convalida come richiesto e StringLength attributi.</span><span class="sxs-lookup"><span data-stu-id="34d5f-185">You learned how to use the different types of validator attributes such as the Required and StringLength attributes.</span></span> <span data-ttu-id="34d5f-186">Inoltre appreso come usare questi attributi quando si lavora sul Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="34d5f-186">You also learned how to use these attributes when working with the Microsoft Entity Framework.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="34d5f-187">[Precedente](validating-with-a-service-layer-cs.md)
> [Successivo](creating-model-classes-with-the-entity-framework-vb.md)</span><span class="sxs-lookup"><span data-stu-id="34d5f-187">[Previous](validating-with-a-service-layer-cs.md)
[Next](creating-model-classes-with-the-entity-framework-vb.md)</span></span>
