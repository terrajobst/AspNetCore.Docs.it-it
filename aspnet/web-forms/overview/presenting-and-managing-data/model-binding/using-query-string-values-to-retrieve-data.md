---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Utilizzo di valori di stringa di query per filtrare i dati con l'associazione di modelli e web form | Documenti Microsoft
author: tfitzmac
description: Questa serie di esercitazioni illustra gli aspetti di base dell'utilizzo di associazione del modello con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 03d20decf0eeff6062fbc6f8dd66f644b405c7cc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886823"
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a><span data-ttu-id="5b8ab-104">Utilizzo di valori di stringa di query per filtrare i dati con l'associazione di modelli e web form</span><span class="sxs-lookup"><span data-stu-id="5b8ab-104">Using query string values to filter data with model binding and web forms</span></span>
====================
<span data-ttu-id="5b8ab-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5b8ab-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5b8ab-106">Questa serie di esercitazioni illustra gli aspetti di base dell'utilizzo di associazione del modello con un progetto di Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="5b8ab-107">Associazione di modelli consente l'interazione dei dati più semplice rispetto a gestione dati di oggetti di origine (ad esempio ObjectDataSource o SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="5b8ab-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="5b8ab-108">Questa serie inizia con materiale introduttivo e sposta concetti più avanzati nelle esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="5b8ab-109">In questa esercitazione viene illustrato come passare un valore nella stringa di query e utilizzare tale valore per recuperare i dati tramite l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-109">This tutorial shows how to pass a value in the query string and use that value to retrieve data through model binding.</span></span>
> 
> <span data-ttu-id="5b8ab-110">Questa esercitazione si basa sul progetto creato nel [precedenti](retrieving-data.md) parti della serie.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-110">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="5b8ab-111">È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in c# o VB.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="5b8ab-112">Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="5b8ab-113">Usa il modello di Visual Studio 2012, è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="5b8ab-114">Occorre invece compilare</span><span class="sxs-lookup"><span data-stu-id="5b8ab-114">What you'll build</span></span>

<span data-ttu-id="5b8ab-115">In questa esercitazione, effettuare le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="5b8ab-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="5b8ab-116">Aggiungere una nuova pagina per mostrare i corsi registrati per uno studente</span><span class="sxs-lookup"><span data-stu-id="5b8ab-116">Add a new page to show the enrolled courses for a student</span></span>
2. <span data-ttu-id="5b8ab-117">Recuperare i corsi registrati per gli studenti selezionato in base a un valore nella stringa di query</span><span class="sxs-lookup"><span data-stu-id="5b8ab-117">Retrieve the enrolled courses for the selected student based on a value in the query string</span></span>
3. <span data-ttu-id="5b8ab-118">Aggiungere un collegamento ipertestuale con un valore di stringa di query dalla visualizzazione griglia alla nuova pagina</span><span class="sxs-lookup"><span data-stu-id="5b8ab-118">Add a hyperlink with a query string value from the grid view to the new page</span></span>

<span data-ttu-id="5b8ab-119">I passaggi in questa esercitazione sono abbastanza simili a quello eseguito in precedenza [esercitazione](sorting-paging-and-filtering-data.md) per filtrare gli studenti visualizzati in base alla selezione utente nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-119">The steps in this tutorial are fairly similar to what you did in the earlier [tutorial](sorting-paging-and-filtering-data.md) to filter the displayed students based on the user selection in a drop down list.</span></span> <span data-ttu-id="5b8ab-120">In tale esercitazione, è stato utilizzato il **controllo** attributo del metodo select per specificare che il valore del parametro provenga da un controllo.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-120">In that tutorial, you used the **Control** attribute in the select method to specify that the parameter value comes from a control.</span></span> <span data-ttu-id="5b8ab-121">In questa esercitazione si userà il **QueryString** attributo del metodo select per specificare che il valore del parametro proviene dalla stringa di query.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-121">In this tutorial, you'll use the **QueryString** attribute in the select method to specify that the parameter value comes from the query string.</span></span>

## <a name="add-new-page-for-displaying-a-students-courses"></a><span data-ttu-id="5b8ab-122">Aggiungere una nuova pagina per visualizzare un corsi</span><span class="sxs-lookup"><span data-stu-id="5b8ab-122">Add new page for displaying a student's courses</span></span>

<span data-ttu-id="5b8ab-123">Aggiungere un nuovo web form che utilizza la pagina master Site. master e denominare la pagina **corsi**.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-123">Add a new web form that uses the Site.master master page, and name the page **Courses**.</span></span>

<span data-ttu-id="5b8ab-124">Nel **Courses.aspx** file, aggiungere una visualizzazione griglia per visualizzare i corsi per studente selezionato.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-124">In the **Courses.aspx** file, add a grid view to display the courses for the selected student.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a><span data-ttu-id="5b8ab-125">Definire il metodo select</span><span class="sxs-lookup"><span data-stu-id="5b8ab-125">Define the select method</span></span>

<span data-ttu-id="5b8ab-126">In **Courses.aspx.cs**, si aggiungerà il metodo select con il nome specificato nella visualizzazione griglia **SelectMethod** proprietà.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-126">In **Courses.aspx.cs**, you will add the select method with the name you specified in the grid view's **SelectMethod** property.</span></span> <span data-ttu-id="5b8ab-127">In quel metodo, verrà definito la query per il recupero di un corsi e specificare che il parametro proviene da un valore di stringa di query con lo stesso nome come parametro.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-127">In that method, you'll define the query for retrieving a student's courses, and specify that the parameter comes from a query string value with the same name as the parameter.</span></span>

<span data-ttu-id="5b8ab-128">In primo luogo, è necessario aggiungere la seguente **utilizzando** istruzioni.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-128">First, you must add the following **using** statements.</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

<span data-ttu-id="5b8ab-129">Quindi, aggiungere il codice seguente Courses.aspx.cs:</span><span class="sxs-lookup"><span data-stu-id="5b8ab-129">Then, add the following code to Courses.aspx.cs:</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

<span data-ttu-id="5b8ab-130">L'attributo di stringa di query indica che un valore di stringa di query denominato StudentID viene automaticamente assegnato al parametro di questo metodo.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-130">The QueryString attribute means that a query string value named StudentID is automatically assigned to the parameter in this method.</span></span>

## <a name="add-hyperlink-with-query-string-value"></a><span data-ttu-id="5b8ab-131">Aggiungere un collegamento ipertestuale con il valore di stringa di query</span><span class="sxs-lookup"><span data-stu-id="5b8ab-131">Add hyperlink with query string value</span></span>

<span data-ttu-id="5b8ab-132">Nella visualizzazione griglia in Students.aspx, si aggiungerà un campo collegamento ipertestuale che si collega alla nuova pagina corsi.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-132">In the grid view on Students.aspx, you will add a hyperlink field that links to your new Courses page.</span></span> <span data-ttu-id="5b8ab-133">Il collegamento ipertestuale includerà un valore di stringa di query con l'id dello studente.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-133">The hyperlink will include a query string value with the student's id.</span></span>

<span data-ttu-id="5b8ab-134">In Students.aspx, aggiungere il seguente campo per le colonne della vista griglia sotto il campo per crediti totale.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-134">In Students.aspx, add the following field to the grid view columns just below the field for Total Credits.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

<span data-ttu-id="5b8ab-135">Eseguire l'applicazione e si noti che la visualizzazione griglia include ora il collegamento di corsi.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-135">Run the application and notice that the grid view now includes the Courses link.</span></span>

![Aggiungere un collegamento ipertestuale](using-query-string-values-to-retrieve-data/_static/image1.png)

<span data-ttu-id="5b8ab-137">Quando si fa clic su uno dei collegamenti, si noterà che registrati corsi.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-137">When you click one of the links, you'll see that student's enrolled courses.</span></span>

![mostrare i corsi](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="5b8ab-139">Conclusione</span><span class="sxs-lookup"><span data-stu-id="5b8ab-139">Conclusion</span></span>

<span data-ttu-id="5b8ab-140">In questa esercitazione, è stato aggiunto un collegamento con un valore di stringa di query.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-140">In this tutorial, you added a link with a query string value.</span></span> <span data-ttu-id="5b8ab-141">Il valore di stringa di query è utilizzato per il valore del parametro del metodo select.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-141">You used that query string value for the parameter value in the select method.</span></span>

<span data-ttu-id="5b8ab-142">Nella prossima [esercitazione](adding-business-logic-layer.md), si sposta il codice dal file code-behind in un livello di logica di business e un livello di accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="5b8ab-142">In the next [tutorial](adding-business-logic-layer.md), you will move the code from the code-behind files into a business logic layer and a data access layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5b8ab-143">[Precedente](integrating-jquery-ui.md)
> [Successivo](adding-business-logic-layer.md)</span><span class="sxs-lookup"><span data-stu-id="5b8ab-143">[Previous](integrating-jquery-ui.md)
[Next](adding-business-logic-layer.md)</span></span>
