---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Ordinamento, paging e filtro dei dati con l'associazione di modelli e web form | Documenti Microsoft
author: tfitzmac
description: Questa serie di esercitazioni illustra gli aspetti di base dell'utilizzo di associazione del modello con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: d63ebecadd392877e4cb1d1dffe9db2d1d231190
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885406"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a><span data-ttu-id="a77a2-104">Ordinamento, paging e filtro dei dati con l'associazione di modelli e web form</span><span class="sxs-lookup"><span data-stu-id="a77a2-104">Sorting, paging, and filtering data with model binding and web forms</span></span>
====================
<span data-ttu-id="a77a2-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a77a2-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a77a2-106">Questa serie di esercitazioni illustra gli aspetti di base dell'utilizzo di associazione del modello con un progetto di Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a77a2-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="a77a2-107">Associazione di modelli consente l'interazione dei dati più semplice rispetto a gestione dati di oggetti di origine (ad esempio ObjectDataSource o SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="a77a2-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="a77a2-108">Questa serie inizia con materiale introduttivo e sposta concetti più avanzati nelle esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="a77a2-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="a77a2-109">In questa esercitazione viene illustrato come aggiungere l'ordinamento, paging e filtro dei dati tramite l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="a77a2-109">This tutorial shows how to add sorting, paging, and filtering of the data through model binding.</span></span>
> 
> <span data-ttu-id="a77a2-110">Questa esercitazione si basa sul progetto creato nel primo [parte](retrieving-data.md) della serie.</span><span class="sxs-lookup"><span data-stu-id="a77a2-110">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="a77a2-111">È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in c# o VB.</span><span class="sxs-lookup"><span data-stu-id="a77a2-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="a77a2-112">Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="a77a2-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="a77a2-113">Usa il modello di Visual Studio 2012, è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="a77a2-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="a77a2-114">Occorre invece compilare</span><span class="sxs-lookup"><span data-stu-id="a77a2-114">What you'll build</span></span>

<span data-ttu-id="a77a2-115">In questa esercitazione, effettuare le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="a77a2-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="a77a2-116">Abilitare l'ordinamento e paging dei dati</span><span class="sxs-lookup"><span data-stu-id="a77a2-116">Enable sorting and paging of the data</span></span>
2. <span data-ttu-id="a77a2-117">Abilitare il filtro dei dati basati su una selezione dall'utente</span><span class="sxs-lookup"><span data-stu-id="a77a2-117">Enable filtering of the data based on a selection by the user</span></span>

## <a name="add-sorting"></a><span data-ttu-id="a77a2-118">Aggiungere l'ordinamento</span><span class="sxs-lookup"><span data-stu-id="a77a2-118">Add sorting</span></span>

<span data-ttu-id="a77a2-119">Abilitazione di un ordinamento in GridView è molto semplice.</span><span class="sxs-lookup"><span data-stu-id="a77a2-119">Enabling sorting in the GridView is very easy.</span></span> <span data-ttu-id="a77a2-120">Nel file Student.aspx, è sufficiente impostare **AllowSorting** a **true** in GridView.</span><span class="sxs-lookup"><span data-stu-id="a77a2-120">In the Student.aspx file, simply set **AllowSorting** to **true** in the GridView.</span></span> <span data-ttu-id="a77a2-121">Non è necessario impostare un **SortExpression** come DataField viene usato automaticamente il valore per ogni colonna.</span><span class="sxs-lookup"><span data-stu-id="a77a2-121">You do not need to set a **SortExpression** value for each column as the DataField is automatically used.</span></span> <span data-ttu-id="a77a2-122">GridView modifica la query per includere l'ordinamento dei dati dal valore selezionato.</span><span class="sxs-lookup"><span data-stu-id="a77a2-122">The GridView modifies the query to include ordering the data by the selected value.</span></span> <span data-ttu-id="a77a2-123">Il codice evidenziato di seguito viene illustrata l'aggiunta che è necessario apportare abilitare l'ordinamento.</span><span class="sxs-lookup"><span data-stu-id="a77a2-123">The highlighted code below shows the addition you need to make to enable sorting.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

<span data-ttu-id="a77a2-124">Eseguire l'applicazione web e test record studente ordinamento per i valori in colonne diverse.</span><span class="sxs-lookup"><span data-stu-id="a77a2-124">Run the web application, and test sorting student records by the values in different columns.</span></span>

![studenti di ordinamento](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a><span data-ttu-id="a77a2-126">aggiunta di impaginazione</span><span class="sxs-lookup"><span data-stu-id="a77a2-126">Add paging</span></span>

<span data-ttu-id="a77a2-127">L'abilitazione di paging è anche molto semplice.</span><span class="sxs-lookup"><span data-stu-id="a77a2-127">Enabling paging is also very easy.</span></span> <span data-ttu-id="a77a2-128">In GridView, impostare il **AllowPaging** proprietà **true** e impostare il **PageSize** proprietà per il numero di record che si desidera visualizzare in ogni pagina.</span><span class="sxs-lookup"><span data-stu-id="a77a2-128">In the GridView, set the **AllowPaging** property to **true** and set the **PageSize** property to the number of records you wish to display on each page.</span></span> <span data-ttu-id="a77a2-129">In questa esercitazione, è possibile impostarla su 4.</span><span class="sxs-lookup"><span data-stu-id="a77a2-129">In this tutorial, you can set it to 4.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

<span data-ttu-id="a77a2-130">Eseguire l'applicazione web e notare che ora i record sono suddivisi su più pagine con non più di 4 record visualizzati in una singola pagina.</span><span class="sxs-lookup"><span data-stu-id="a77a2-130">Run the web application, and notice that now the records are divided over multiple pages with no more than 4 records displayed on a single page.</span></span>

![aggiunta di impaginazione](sorting-paging-and-filtering-data/_static/image4.png)

<span data-ttu-id="a77a2-132">Esecuzione di query posticipata migliora l'efficienza dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a77a2-132">Deferred query execution improves the application efficiency.</span></span> <span data-ttu-id="a77a2-133">Invece di recuperare l'intero set di dati, il controllo GridView modifica la query per recuperare solo i record per la pagina corrente.</span><span class="sxs-lookup"><span data-stu-id="a77a2-133">Instead of retrieving the entire data set, the GridView modifies the query to retrieve only the records for the current page.</span></span>

## <a name="filter-records-by-user-selection"></a><span data-ttu-id="a77a2-134">Filtrare i record per la selezione utente</span><span class="sxs-lookup"><span data-stu-id="a77a2-134">Filter records by user selection</span></span>

<span data-ttu-id="a77a2-135">Associazione di modelli aggiunge diversi attributi che consentono di designare come impostare il valore per un parametro in un metodo di associazione del modello.</span><span class="sxs-lookup"><span data-stu-id="a77a2-135">Model binding adds several attributes which enable you to designate how to set the value for a parameter in a model binding method.</span></span> <span data-ttu-id="a77a2-136">Questi attributi sono di **System.Web.ModelBinding** dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="a77a2-136">These attributes are in the **System.Web.ModelBinding** namespace.</span></span> <span data-ttu-id="a77a2-137">e comprendono:</span><span class="sxs-lookup"><span data-stu-id="a77a2-137">They include:</span></span>

- <span data-ttu-id="a77a2-138">Control</span><span class="sxs-lookup"><span data-stu-id="a77a2-138">Control</span></span>
- <span data-ttu-id="a77a2-139">Cookie</span><span class="sxs-lookup"><span data-stu-id="a77a2-139">Cookie</span></span>
- <span data-ttu-id="a77a2-140">Form</span><span class="sxs-lookup"><span data-stu-id="a77a2-140">Form</span></span>
- <span data-ttu-id="a77a2-141">Profilo</span><span class="sxs-lookup"><span data-stu-id="a77a2-141">Profile</span></span>
- <span data-ttu-id="a77a2-142">QueryString</span><span class="sxs-lookup"><span data-stu-id="a77a2-142">QueryString</span></span>
- <span data-ttu-id="a77a2-143">RouteData</span><span class="sxs-lookup"><span data-stu-id="a77a2-143">RouteData</span></span>
- <span data-ttu-id="a77a2-144">Sessione</span><span class="sxs-lookup"><span data-stu-id="a77a2-144">Session</span></span>
- <span data-ttu-id="a77a2-145">UserProfile</span><span class="sxs-lookup"><span data-stu-id="a77a2-145">UserProfile</span></span>
- <span data-ttu-id="a77a2-146">ViewState</span><span class="sxs-lookup"><span data-stu-id="a77a2-146">ViewState</span></span>

<span data-ttu-id="a77a2-147">In questa esercitazione si utilizzerà il valore di un controllo per filtrare i record visualizzati in GridView.</span><span class="sxs-lookup"><span data-stu-id="a77a2-147">In this tutorial, you will use a control's value to filter which records are displayed in the GridView.</span></span> <span data-ttu-id="a77a2-148">Si aggiungerà il **controllo** attributo al metodo di query è stato creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="a77a2-148">You will add the **Control** attribute to the query method you had created earlier.</span></span> <span data-ttu-id="a77a2-149">In un [in un secondo momento](using-query-string-values-to-retrieve-data.md) esercitazione, si applicherà il **QueryString** attributo a un parametro per specificare che il valore del parametro proviene da un valore di stringa di query.</span><span class="sxs-lookup"><span data-stu-id="a77a2-149">In a [later](using-query-string-values-to-retrieve-data.md) tutorial, you will apply the **QueryString** attribute to a parameter to specify that the parameter value comes from a query string value.</span></span>

<span data-ttu-id="a77a2-150">In primo luogo, sopra il controllo ValidationSummary, aggiungere un menu a discesa per il filtro vengono visualizzati quali studenti.</span><span class="sxs-lookup"><span data-stu-id="a77a2-150">First, above the ValidationSummary, add a drop down list for filtering which students are shown.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

<span data-ttu-id="a77a2-151">Nel file code-behind, modificare il metodo select per ricevere un valore dal controllo e impostare il nome del parametro sul nome del controllo che fornisce il valore.</span><span class="sxs-lookup"><span data-stu-id="a77a2-151">In the code-behind file, modify the select method to receive a value from the control, and set the name of the parameter to the name of the control that provides the value.</span></span>

<span data-ttu-id="a77a2-152">È necessario aggiungere un **utilizzando** istruzione per il **System.Web.ModelBinding** dello spazio dei nomi per risolvere l'attributo del controllo.</span><span class="sxs-lookup"><span data-stu-id="a77a2-152">You must add a **using** statement for the **System.Web.ModelBinding** namespace to resolve the Control attribute.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

<span data-ttu-id="a77a2-153">Il codice seguente illustra il metodo select nuovamente lavorato per filtrare i dati restituiti in base al valore dell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="a77a2-153">The following code shows the select method re-worked to filter the returned data based on the value of the drop down list.</span></span> <span data-ttu-id="a77a2-154">Aggiunta di un attributo di controllo prima di un parametro specifica che il valore per questo parametro viene fornito da un controllo con lo stesso nome.</span><span class="sxs-lookup"><span data-stu-id="a77a2-154">Adding a control attribute before a parameter specifies that the value for this parameter comes from a control with the same name.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

<span data-ttu-id="a77a2-155">Eseguire l'applicazione web e selezionare valori diversi nell'elenco a discesa elenco per filtrare l'elenco di studenti.</span><span class="sxs-lookup"><span data-stu-id="a77a2-155">Run the web application and select different values from the drop down list to filter the list of students.</span></span>

![studenti di filtro](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a><span data-ttu-id="a77a2-157">Conclusione</span><span class="sxs-lookup"><span data-stu-id="a77a2-157">Conclusion</span></span>

<span data-ttu-id="a77a2-158">In questa esercitazione è abilitato ordinamento e paging dei dati.</span><span class="sxs-lookup"><span data-stu-id="a77a2-158">In this tutorial, you enabled sorting and paging of the data.</span></span> <span data-ttu-id="a77a2-159">È attivato il filtro dei dati dal valore di un controllo.</span><span class="sxs-lookup"><span data-stu-id="a77a2-159">You also enabled filtering the data by the value of a control.</span></span>

<span data-ttu-id="a77a2-160">Nella prossima [esercitazione](integrating-jquery-ui.md) si procederà al miglioramento dell'interfaccia utente tramite l'integrazione di un widget di JQuery UI nel modello di dati dinamica.</span><span class="sxs-lookup"><span data-stu-id="a77a2-160">In the next [tutorial](integrating-jquery-ui.md) you will enhance the UI by integrating a JQuery UI widget into the dynamic data template.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a77a2-161">[Precedente](updating-deleting-and-creating-data.md)
> [Successivo](integrating-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="a77a2-161">[Previous](updating-deleting-and-creating-data.md)
[Next](integrating-jquery-ui.md)</span></span>
