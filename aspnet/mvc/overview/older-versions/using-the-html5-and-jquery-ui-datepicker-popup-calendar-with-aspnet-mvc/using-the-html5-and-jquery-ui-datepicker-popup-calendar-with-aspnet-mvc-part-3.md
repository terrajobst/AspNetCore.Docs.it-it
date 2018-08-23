---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Uso di HTML5 e jQuery UI Datepicker Popup Calendar con ASP.NET MVC - parte 3 | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione insegnerà le nozioni di base di come usare i modelli nell'editor di modelli di visualizzazione e il calendario jQuery UI datepicker popup in MV un ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: 184d93323ac6f2cb7f14d32b401cf6a400eb79a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835296"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a><span data-ttu-id="2ab62-103">Uso di HTML5 e jQuery UI Datepicker Popup Calendar con ASP.NET MVC - parte 3</span><span class="sxs-lookup"><span data-stu-id="2ab62-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 3</span></span>
====================
<span data-ttu-id="2ab62-104">da [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="2ab62-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="2ab62-105">Questa esercitazione insegnerà le nozioni di base di come usare i modelli nell'editor di modelli di visualizzazione e il calendario di jQuery UI datepicker popup in un'applicazione Web ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2ab62-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>


## <a name="working-with-complex-types"></a><span data-ttu-id="2ab62-106">Utilizzo di tipi complessi</span><span class="sxs-lookup"><span data-stu-id="2ab62-106">Working with Complex Types</span></span>

<span data-ttu-id="2ab62-107">In questa sezione si verrà creato una classe indirizzo e informazioni su come creare un modello per visualizzarlo.</span><span class="sxs-lookup"><span data-stu-id="2ab62-107">In this section you'll create an address class and learn how to create a template to display it.</span></span>

<span data-ttu-id="2ab62-108">Nel *modelli* cartella, creare un nuovo file di classe denominato *Person.cs* in cui verrà inserito due tipi: una `Person` classe e un `Address` classe.</span><span class="sxs-lookup"><span data-stu-id="2ab62-108">In the *Models* folder, create a new class file named *Person.cs* where you'll put two types: a `Person` class and an `Address` class.</span></span> <span data-ttu-id="2ab62-109">Il `Person` contiene una proprietà tipizzata come classe `Address`.</span><span class="sxs-lookup"><span data-stu-id="2ab62-109">The `Person` class will contain a property that's typed as `Address`.</span></span> <span data-ttu-id="2ab62-110">Il `Address` tipo è un tipo complesso, vale a dire non è uno dei tipi incorporati, ad esempio `int`, `string`, o `double`.</span><span class="sxs-lookup"><span data-stu-id="2ab62-110">The `Address` type is a complex type, meaning it's not one of the built-in types like `int`, `string`, or `double`.</span></span> <span data-ttu-id="2ab62-111">Ma ne contiene diverse proprietà.</span><span class="sxs-lookup"><span data-stu-id="2ab62-111">Instead, it has several properties.</span></span> <span data-ttu-id="2ab62-112">Il codice per le nuove classi è simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="2ab62-112">The code for the new classes looks like this:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

<span data-ttu-id="2ab62-113">Nel `Movie` controller, aggiungere il codice seguente `PersonDetail` azione per visualizzare un'istanza di persona:</span><span class="sxs-lookup"><span data-stu-id="2ab62-113">In the `Movie` controller, add the following `PersonDetail` action to display a person instance:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

<span data-ttu-id="2ab62-114">Quindi aggiungere il codice seguente per il `Movie` controller per popolare il `Person` modello con alcuni dati di esempio:</span><span class="sxs-lookup"><span data-stu-id="2ab62-114">Then add the following code to the `Movie` controller to populate the `Person` model with some sample data:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

<span data-ttu-id="2ab62-115">Aprire il *Views\Movies\PersonDetail.cshtml* file e aggiungere il markup seguente per il `PersonDetail` vista.</span><span class="sxs-lookup"><span data-stu-id="2ab62-115">Open the *Views\Movies\PersonDetail.cshtml* file and add the following markup for the `PersonDetail` view.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

<span data-ttu-id="2ab62-116">Premere CTRL+F5 per eseguire l'applicazione e passare a *Movies/PersonDetail*.</span><span class="sxs-lookup"><span data-stu-id="2ab62-116">Press Ctrl+F5 to run the application and navigate to *Movies/PersonDetail*.</span></span>

<span data-ttu-id="2ab62-117">Il `PersonDetail` vista non contiene il `Address` tipo complesso, come illustrato in questo screenshot.</span><span class="sxs-lookup"><span data-stu-id="2ab62-117">The `PersonDetail` view doesn't contain the `Address` complex type, as you can see in this screenshot.</span></span> <span data-ttu-id="2ab62-118">(Nessun indirizzo figura).</span><span class="sxs-lookup"><span data-stu-id="2ab62-118">(No address is shown.)</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

<span data-ttu-id="2ab62-119">Il `Address` i dati del modello non viene visualizzati perché è un tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="2ab62-119">The `Address` model data is not displayed because it's a complex type.</span></span> <span data-ttu-id="2ab62-120">Per visualizzare le informazioni sull'indirizzo, aprire il *Views\Movies\PersonDetail.cshtml* file nuovo e aggiungere il markup seguente.</span><span class="sxs-lookup"><span data-stu-id="2ab62-120">To display the address information, open the *Views\Movies\PersonDetail.cshtml* file again and add the following markup.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

<span data-ttu-id="2ab62-121">Il markup completo per il `PersonDetail` ora Vista si presenta come segue:</span><span class="sxs-lookup"><span data-stu-id="2ab62-121">The complete markup for the `PersonDetail` now view looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

<span data-ttu-id="2ab62-122">Eseguire nuovamente l'applicazione e visualizzare il `PersonDetail` vista.</span><span class="sxs-lookup"><span data-stu-id="2ab62-122">Run the application again and display the `PersonDetail` view.</span></span> <span data-ttu-id="2ab62-123">Vengono ora visualizzate le informazioni di indirizzo:</span><span class="sxs-lookup"><span data-stu-id="2ab62-123">The address information is now displayed:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a><span data-ttu-id="2ab62-124">Creazione di un modello per un tipo complesso</span><span class="sxs-lookup"><span data-stu-id="2ab62-124">Creating a Template for a Complex Type</span></span>

<span data-ttu-id="2ab62-125">In questa sezione si creerà un modello che verrà usato per eseguire il rendering di `Address` tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="2ab62-125">In this section you'll create a template that will be used to render the `Address` complex type.</span></span> <span data-ttu-id="2ab62-126">Quando si crea un modello per il `Address` tipo, ASP.NET MVC può automaticamente utilizzarlo per formattare un modello di indirizzo in un punto qualsiasi nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2ab62-126">When you create a template for the `Address` type, ASP.NET MVC can automatically use it to format an address model anywhere in the application.</span></span> <span data-ttu-id="2ab62-127">Ciò offre un modo per controllare il rendering del `Address` tipo da un'unica posizione nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2ab62-127">This gives you a way to control the rendering of the `Address` type from just one place in the application.</span></span>

<span data-ttu-id="2ab62-128">Nel *Views\Shared\DisplayTemplates.* cartella, creare una visualizzazione parziale fortemente tipizzata denominata **indirizzo**:</span><span class="sxs-lookup"><span data-stu-id="2ab62-128">In the *Views\Shared\DisplayTemplates* folder, create a strongly typed partial view named **Address**:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

<span data-ttu-id="2ab62-129">Fare clic su **Add**, quindi aprire il nuovo *Views\Shared\DisplayTemplates\Address.cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="2ab62-129">Click **Add**, and then open the new *Views\Shared\DisplayTemplates\Address.cshtml* file.</span></span> <span data-ttu-id="2ab62-130">La nuova visualizzazione contiene il markup generato seguente:</span><span class="sxs-lookup"><span data-stu-id="2ab62-130">The new view contains the following generated markup:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

<span data-ttu-id="2ab62-131">Eseguire l'applicazione e visualizzare il `PersonDetail` vista.</span><span class="sxs-lookup"><span data-stu-id="2ab62-131">Run the application and display the `PersonDetail` view.</span></span> <span data-ttu-id="2ab62-132">Questa volta, il `Address` modello appena creato viene usato per visualizzare il `Address` tipo complesso, in modo che la visualizzazione è simile a quanto segue:</span><span class="sxs-lookup"><span data-stu-id="2ab62-132">This time, the `Address` template that you just created is used to display the `Address` complex type, so the display looks like the following:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a><span data-ttu-id="2ab62-133">Riepilogo: Modi per specificare il formato di visualizzazione del modello e il modello</span><span class="sxs-lookup"><span data-stu-id="2ab62-133">Summary: Ways to Specify the Model Display Format and Template</span></span>

<span data-ttu-id="2ab62-134">Si è visto che è possibile specificare il formato o modello per una proprietà del modello utilizzando gli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="2ab62-134">You've seen that you can specify the format or template for a model property by using the following approaches:</span></span>

- <span data-ttu-id="2ab62-135">Applicando la `DisplayFormat` attributo a una proprietà nel modello.</span><span class="sxs-lookup"><span data-stu-id="2ab62-135">Applying the `DisplayFormat` attribute to a property in the model.</span></span> <span data-ttu-id="2ab62-136">Ad esempio, il codice seguente causa la data da visualizzare senza l'ora:</span><span class="sxs-lookup"><span data-stu-id="2ab62-136">For example, the following code causes the date to be displayed without the time:</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- <span data-ttu-id="2ab62-137">Applicazione di un [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attributo a una proprietà nel modello e specificare il tipo di dati.</span><span class="sxs-lookup"><span data-stu-id="2ab62-137">Applying a [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute to a property in the model and specifying the data type.</span></span> <span data-ttu-id="2ab62-138">Ad esempio, il codice seguente fa sì che la data da visualizzare senza l'ora.</span><span class="sxs-lookup"><span data-stu-id="2ab62-138">For example, the following code causes the date to be displayed without the time.</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    <span data-ttu-id="2ab62-139">Se l'applicazione contiene un *date.cshtml* modello nel *Views\Shared\DisplayTemplates.* cartella o nella *Views\Movies\DisplayTemplates* cartella, tale modello verrà usato per eseguire il rendering di `DateTime` proprietà.</span><span class="sxs-lookup"><span data-stu-id="2ab62-139">If the application contains a *date.cshtml* template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder, that template will be used to render the `DateTime` property.</span></span> <span data-ttu-id="2ab62-140">Il sistema di creazione modello ASP.NET predefinito in caso contrario, viene visualizzata la proprietà come Data.</span><span class="sxs-lookup"><span data-stu-id="2ab62-140">Otherwise the built-in ASP.NET templating system displays the property as a date.</span></span>
- <span data-ttu-id="2ab62-141">Creazione di un modello di visualizzazione nel *Views\Shared\DisplayTemplates.* cartella o il *Views\Movies\DisplayTemplates* cartella il cui nome corrisponde al tipo di dati che si desidera formattare.</span><span class="sxs-lookup"><span data-stu-id="2ab62-141">Creating a display template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder whose name matches the data type that you want to format.</span></span> <span data-ttu-id="2ab62-142">Ad esempio, si è visto che il *Views\Shared\DisplayTemplates\DateTime.cshtml* utilizzato per eseguire il rendering `DateTime` proprietà in un modello, senza l'aggiunta di un attributo nel modello e senza l'aggiunta di eventuali tag alle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="2ab62-142">For example, you saw that the *Views\Shared\DisplayTemplates\DateTime.cshtml* was used to render `DateTime` properties in a model, without adding an attribute to the model and without adding any markup to views.</span></span>
- <span data-ttu-id="2ab62-143">Usando il [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attributo sul modello per specificare il modello per visualizzare le proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="2ab62-143">Using the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute on the model to specify the template to display the model property.</span></span>
- <span data-ttu-id="2ab62-144">Aggiungere in modo esplicito il nome del modello di visualizzazione per il [HTML. DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) chiamare in una vista.</span><span class="sxs-lookup"><span data-stu-id="2ab62-144">Explicitly adding the display template name to the [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) call in a view.</span></span>

<span data-ttu-id="2ab62-145">L'approccio che si utilizza varia a seconda cosa da eseguire nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2ab62-145">The approach you use depends on what you need to do in your application.</span></span> <span data-ttu-id="2ab62-146">Non è insolito combinare questi approcci per ottenere esattamente il tipo di formattazione che è necessario.</span><span class="sxs-lookup"><span data-stu-id="2ab62-146">It's not uncommon to mix these approaches to get exactly the kind of formatting that you need.</span></span>

<span data-ttu-id="2ab62-147">Nella sezione successiva, sarà possibile passare un po' analizzato e spostare da personalizzare la modalità di visualizzazione per personalizzare la modalità di immissione dati.</span><span class="sxs-lookup"><span data-stu-id="2ab62-147">In the next section, you'll switch gears a bit and move from customizing how data is displayed to customizing how it's entered.</span></span> <span data-ttu-id="2ab62-148">Sarà associare il controllo datepicker jQuery per le viste modifica dell'applicazione per fornire un buon metodo per specificare le date.</span><span class="sxs-lookup"><span data-stu-id="2ab62-148">You'll hook up the jQuery datepicker to the edit views in the application in order to provide a slick way to specify dates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2ab62-149">[Precedente](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Successivo](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="2ab62-149">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span></span>
