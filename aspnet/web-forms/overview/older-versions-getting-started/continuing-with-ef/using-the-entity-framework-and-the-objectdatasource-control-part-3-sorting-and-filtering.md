---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Utilizzo di Entity Framework 4.0 e controllo ObjectDataSource, parte 3: ordinamento e filtro | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni si basa sull'applicazione web di Contoso University specificano che viene creato da Getting Started with serie di esercitazioni in Entity Framework 4.0. POSSO...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 48d859191877ba951e233f19873d52625fe180ac
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378910"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a><span data-ttu-id="35287-104">Utilizzo di Entity Framework 4.0 e controllo ObjectDataSource, parte 3: ordinamento e filtro</span><span class="sxs-lookup"><span data-stu-id="35287-104">Using the Entity Framework 4.0 and the ObjectDataSource Control, Part 3: Sorting and Filtering</span></span>
====================
<span data-ttu-id="35287-105">da [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="35287-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="35287-106">Questa serie di esercitazioni si basa sull'applicazione web Contoso University specificano che viene creato per il [Introduzione a Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) serie di esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="35287-106">This tutorial series builds on the Contoso University web application that is created by the [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) tutorial series.</span></span> <span data-ttu-id="35287-107">Se si non è stato completato le esercitazioni precedenti, come punto di partenza per questa esercitazione è possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) verrebbe creato.</span><span class="sxs-lookup"><span data-stu-id="35287-107">If you didn't complete the earlier tutorials, as a starting point for this tutorial you can [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) that you would have created.</span></span> <span data-ttu-id="35287-108">È anche possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creato dalla serie di esercitazioni complete.</span><span class="sxs-lookup"><span data-stu-id="35287-108">You can also [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) that is created by the complete tutorial series.</span></span> <span data-ttu-id="35287-109">Se si hanno domande sulle esercitazioni, è possibile pubblicarli per i [forum di ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).</span><span class="sxs-lookup"><span data-stu-id="35287-109">If you have questions about the tutorials, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx).</span></span>


<span data-ttu-id="35287-110">Nell'esercitazione precedente è stato implementato il modello di repository in un'applicazione web a più livelli che usa Entity Framework e `ObjectDataSource` controllo.</span><span class="sxs-lookup"><span data-stu-id="35287-110">In the previous tutorial you implemented the repository pattern in an n-tier web application that uses the Entity Framework and the `ObjectDataSource` control.</span></span> <span data-ttu-id="35287-111">Questa esercitazione illustra come eseguire operazioni di ordinamento e filtraggio e gestire gli scenari di master-dettagli.</span><span class="sxs-lookup"><span data-stu-id="35287-111">This tutorial shows how to do sorting and filtering and handle master-detail scenarios.</span></span> <span data-ttu-id="35287-112">Si aggiungeranno i miglioramenti seguenti per il *Departments.aspx* pagina:</span><span class="sxs-lookup"><span data-stu-id="35287-112">You'll add the following enhancements to the *Departments.aspx* page:</span></span>

- <span data-ttu-id="35287-113">Una casella di testo per consentire agli utenti di selezionare i reparti in base al nome.</span><span class="sxs-lookup"><span data-stu-id="35287-113">A text box to allow users to select departments by name.</span></span>
- <span data-ttu-id="35287-114">Un elenco dei corsi per ogni reparto che viene visualizzato nella griglia.</span><span class="sxs-lookup"><span data-stu-id="35287-114">A list of courses for each department that's shown in the grid.</span></span>
- <span data-ttu-id="35287-115">La possibilità di ordinare facendo clic sulle intestazioni di colonna.</span><span class="sxs-lookup"><span data-stu-id="35287-115">The ability to sort by clicking column headings.</span></span>

<span data-ttu-id="35287-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="35287-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span></span>

## <a name="adding-the-ability-to-sort-gridview-columns"></a><span data-ttu-id="35287-117">Aggiunta della capacità Ordina colonne GridView</span><span class="sxs-lookup"><span data-stu-id="35287-117">Adding the Ability to Sort GridView Columns</span></span>

<span data-ttu-id="35287-118">Aprire il *Departments.aspx* pagina e aggiungere un `SortParameterName="sortExpression"` attributo il `ObjectDataSource` controllo denominato `DepartmentsObjectDataSource`.</span><span class="sxs-lookup"><span data-stu-id="35287-118">Open the *Departments.aspx* page and add a `SortParameterName="sortExpression"` attribute to the `ObjectDataSource` control named `DepartmentsObjectDataSource`.</span></span> <span data-ttu-id="35287-119">(In seguito si creerà una `GetDepartments` metodo che accetta un parametro denominato `sortExpression`.) Il markup per il tag di apertura del controllo è ora simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="35287-119">(Later you'll create a `GetDepartments` method that takes a parameter named `sortExpression`.) The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

<span data-ttu-id="35287-120">Aggiungere il `AllowSorting="true"` dell'attributo al tag di apertura del `GridView` controllo.</span><span class="sxs-lookup"><span data-stu-id="35287-120">Add the `AllowSorting="true"` attribute to the opening tag of the `GridView` control.</span></span> <span data-ttu-id="35287-121">Il markup per il tag di apertura del controllo è ora simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="35287-121">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

<span data-ttu-id="35287-122">In *Departments.aspx.cs*, impostare l'ordinamento predefinito chiamando il `GridView` del controllo `Sort` metodo dal `Page_Load` metodo:</span><span class="sxs-lookup"><span data-stu-id="35287-122">In *Departments.aspx.cs*, set the default sort order by calling the `GridView` control's `Sort` method from the `Page_Load` method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

<span data-ttu-id="35287-123">È possibile aggiungere il codice che consente di ordinare o filtra la classe per la logica di business o la classe di repository.</span><span class="sxs-lookup"><span data-stu-id="35287-123">You can add code that sorts or filters in either the business logic class or the repository class.</span></span> <span data-ttu-id="35287-124">Se si fa nella classe della logica aziendale, l'ordinamento o filtro lavoro verrà eseguito dopo che i dati vengono recuperati dal database, perché la classe per la logica di business sta collaborando con un `IEnumerable` oggetto restituito dal repository.</span><span class="sxs-lookup"><span data-stu-id="35287-124">If you do it in the business logic class, the sorting or filtering work will be done after the data is retrieved from the database, because the business logic class is working with an `IEnumerable` object returned by the repository.</span></span> <span data-ttu-id="35287-125">Se si aggiunge l'ordinamento e filtro codice nella classe di repository e effettuata prima di un'espressione LINQ o query di oggetto è stato convertito in un `IEnumerable` dell'oggetto, i comandi verranno passati al database per l'elaborazione, che corrisponde in genere più efficiente.</span><span class="sxs-lookup"><span data-stu-id="35287-125">If you add sorting and filtering code in the repository class and you do it before a LINQ expression or object query has been converted to an `IEnumerable` object, your commands will be passed through to the database for processing, which is typically more efficient.</span></span> <span data-ttu-id="35287-126">In questa esercitazione è implementato l'ordinamento e filtri in modo che causa l'elaborazione da eseguire nel database, vale a dire nel repository.</span><span class="sxs-lookup"><span data-stu-id="35287-126">In this tutorial you'll implement sorting and filtering in a way that causes the processing to be done by the database — that is, in the repository.</span></span>

<span data-ttu-id="35287-127">Per aggiungere funzionalità di ordinamento, è necessario aggiungere un nuovo metodo per l'interfaccia del repository e classi repository anche la classe per la logica di business.</span><span class="sxs-lookup"><span data-stu-id="35287-127">To add sorting capability, you must add a new method to the repository interface and repository classes as well as to the business logic class.</span></span> <span data-ttu-id="35287-128">Nel *ISchoolRepository.cs* , aggiungere un nuovo `GetDepartments` metodo che accetta un `sortExpression` parametro che verrà usato per ordinare l'elenco dei reparti che viene restituito:</span><span class="sxs-lookup"><span data-stu-id="35287-128">In the *ISchoolRepository.cs* file, add a new `GetDepartments` method that takes a `sortExpression` parameter that will be used to sort the list of departments that's returned:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

<span data-ttu-id="35287-129">Il `sortExpression` parametro specificherà la colonna da ordinare e la direzione di ordinamento.</span><span class="sxs-lookup"><span data-stu-id="35287-129">The `sortExpression` parameter will specify the column to sort on and the sort direction.</span></span>

<span data-ttu-id="35287-130">Aggiungere il codice per il nuovo metodo per la *SchoolRepository.cs* file:</span><span class="sxs-lookup"><span data-stu-id="35287-130">Add code for the new method to the *SchoolRepository.cs* file:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

<span data-ttu-id="35287-131">Modificare l'oggetto esistente senza parametri `GetDepartments` metodo da chiamare il nuovo metodo:</span><span class="sxs-lookup"><span data-stu-id="35287-131">Change the existing parameterless `GetDepartments` method to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

<span data-ttu-id="35287-132">Nel progetto di test, aggiungere il nuovo metodo seguente a *MockSchoolRepository.cs*:</span><span class="sxs-lookup"><span data-stu-id="35287-132">In the test project, add the following new method to *MockSchoolRepository.cs*:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

<span data-ttu-id="35287-133">Se si prevede di creare tutti gli unit test che dipendono da questo metodo restituisce un elenco ordinato, è necessario ordinare l'elenco prima di restituirlo.</span><span class="sxs-lookup"><span data-stu-id="35287-133">If you were going to create any unit tests that depended on this method returning a sorted list, you would need to sort the list before returning it.</span></span> <span data-ttu-id="35287-134">È non verranno creati test simili in questa esercitazione, in modo che il metodo può restituire solo l'elenco non ordinato dei reparti.</span><span class="sxs-lookup"><span data-stu-id="35287-134">You won't be creating tests like that in this tutorial, so the method can just return the unsorted list of departments.</span></span>

<span data-ttu-id="35287-135">Nel *SchoolBL.cs* , aggiungere il nuovo metodo seguente alla classe per la logica di business:</span><span class="sxs-lookup"><span data-stu-id="35287-135">In the *SchoolBL.cs* file, add the following new method to the business logic class:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

<span data-ttu-id="35287-136">Questo codice passa il parametro di ordinamento per il metodo di repository.</span><span class="sxs-lookup"><span data-stu-id="35287-136">This code passes the sort parameter to the repository method.</span></span>

<span data-ttu-id="35287-137">Eseguire la *Departments.aspx* pagina.</span><span class="sxs-lookup"><span data-stu-id="35287-137">Run the *Departments.aspx* page.</span></span>

<span data-ttu-id="35287-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="35287-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span></span>

<span data-ttu-id="35287-139">È ora possibile fare clic su un'intestazione di colonna per ordinare in base alla colonna.</span><span class="sxs-lookup"><span data-stu-id="35287-139">You can now click any column heading to sort by that column.</span></span> <span data-ttu-id="35287-140">Se la colonna già ordinata, facendo clic sull'intestazione di inverte la direzione di ordinamento.</span><span class="sxs-lookup"><span data-stu-id="35287-140">If the column is already sorted, clicking the heading reverses the sort direction.</span></span>

## <a name="adding-a-search-box"></a><span data-ttu-id="35287-141">Aggiunta di una casella di ricerca</span><span class="sxs-lookup"><span data-stu-id="35287-141">Adding a Search Box</span></span>

<span data-ttu-id="35287-142">In questa sezione si sarà aggiungere una casella di testo di ricerca, crea un collegamento per il `ObjectDataSource` controllare l'utilizzo di un parametro di controllo e aggiungere un metodo alla classe per la logica di business per supportare il filtro.</span><span class="sxs-lookup"><span data-stu-id="35287-142">In this section you'll add a search text box, link it to the `ObjectDataSource` control using a control parameter, and add a method to the business logic class to support filtering.</span></span>

<span data-ttu-id="35287-143">Aprire il *Departments.aspx* pagina e aggiungere il markup seguente tra i primo e l'intestazione `ObjectDataSource` controllo:</span><span class="sxs-lookup"><span data-stu-id="35287-143">Open the *Departments.aspx* page and add the following markup between the heading and the first `ObjectDataSource` control:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

<span data-ttu-id="35287-144">Nel `ObjectDataSource` controllo denominato `DepartmentsObjectDataSource`, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="35287-144">In the `ObjectDataSource` control named `DepartmentsObjectDataSource`, do the following:</span></span>

- <span data-ttu-id="35287-145">Aggiungere un `SelectParameters` (elemento) per un parametro denominato `nameSearchString` che ottiene il valore immesso nel `SearchTextBox` controllo.</span><span class="sxs-lookup"><span data-stu-id="35287-145">Add a `SelectParameters` element for a parameter named `nameSearchString` that gets the value entered in the `SearchTextBox` control.</span></span>
- <span data-ttu-id="35287-146">Modifica il `SelectMethod` al valore dell'attributo `GetDepartmentsByName`.</span><span class="sxs-lookup"><span data-stu-id="35287-146">Change the `SelectMethod` attribute value to `GetDepartmentsByName`.</span></span> <span data-ttu-id="35287-147">(Verrà creato questo metodo in un secondo momento.)</span><span class="sxs-lookup"><span data-stu-id="35287-147">(You'll create this method later.)</span></span>

<span data-ttu-id="35287-148">Il markup per il `ObjectDataSource` controllo sarà ora simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="35287-148">The markup for the `ObjectDataSource` control now resembles the following example:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

<span data-ttu-id="35287-149">Nelle *ISchoolRepository.cs*, aggiungere un `GetDepartmentsByName` metodo che accetta entrambi `sortExpression` e `nameSearchString` parametri:</span><span class="sxs-lookup"><span data-stu-id="35287-149">In *ISchoolRepository.cs*, add a `GetDepartmentsByName` method that takes both `sortExpression` and `nameSearchString` parameters:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

<span data-ttu-id="35287-150">Nelle *SchoolRepository.cs*, aggiungere il nuovo metodo seguente:</span><span class="sxs-lookup"><span data-stu-id="35287-150">In *SchoolRepository.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

<span data-ttu-id="35287-151">Questo codice Usa un `Where` (metodo) per selezionare gli elementi che contengono la stringa di ricerca.</span><span class="sxs-lookup"><span data-stu-id="35287-151">This code uses a `Where` method to select items that contain the search string.</span></span> <span data-ttu-id="35287-152">Se la stringa di ricerca è vuota, verranno selezionati tutti i record.</span><span class="sxs-lookup"><span data-stu-id="35287-152">If the search string is empty, all records will be selected.</span></span> <span data-ttu-id="35287-153">Si noti che quando si specifica chiamate al metodo insieme in un'istruzione simile alla seguente (`Include`, quindi `OrderBy`, quindi `Where`), il `Where` metodo deve sempre essere l'ultimo.</span><span class="sxs-lookup"><span data-stu-id="35287-153">Note that when you specify method calls together in one statement like this (`Include`, then `OrderBy`, then `Where`), the `Where` method must always be last.</span></span>

<span data-ttu-id="35287-154">Modificare l'oggetto esistente `GetDepartments` metodo che accetta un `sortExpression` parametro per chiamare il nuovo metodo:</span><span class="sxs-lookup"><span data-stu-id="35287-154">Change the existing `GetDepartments` method that takes a `sortExpression` parameter to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

<span data-ttu-id="35287-155">Nelle *MockSchoolRepository.cs* nel progetto di test, aggiungere il nuovo metodo seguente:</span><span class="sxs-lookup"><span data-stu-id="35287-155">In *MockSchoolRepository.cs* in the test project, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

<span data-ttu-id="35287-156">Nelle *SchoolBL.cs*, aggiungere il nuovo metodo seguente:</span><span class="sxs-lookup"><span data-stu-id="35287-156">In *SchoolBL.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

<span data-ttu-id="35287-157">Eseguire la *Departments.aspx* pagina e immettere una stringa di ricerca per assicurarsi che la logica di selezione funziona.</span><span class="sxs-lookup"><span data-stu-id="35287-157">Run the *Departments.aspx* page and enter a search string to make sure that the selection logic works.</span></span> <span data-ttu-id="35287-158">Lasciare vuota la casella di testo e provare a eseguire una ricerca per assicurarsi che tutti i record vengono restituiti.</span><span class="sxs-lookup"><span data-stu-id="35287-158">Leave the text box empty and try a search to make sure that all records are returned.</span></span>

<span data-ttu-id="35287-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="35287-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span></span>

## <a name="adding-a-details-column-for-each-grid-row"></a><span data-ttu-id="35287-160">Aggiunta di una colonna dei dettagli per ogni riga della griglia</span><span class="sxs-lookup"><span data-stu-id="35287-160">Adding a Details Column for Each Grid Row</span></span>

<span data-ttu-id="35287-161">Successivamente, si desidera visualizzare tutti i corsi per ogni reparto visualizzato nella cella a destra della griglia.</span><span class="sxs-lookup"><span data-stu-id="35287-161">Next, you want to see all of the courses for each department displayed in the right-hand cell of the grid.</span></span> <span data-ttu-id="35287-162">A tale scopo, si userà un annidato `GridView` controllo e associa i dati per i dati dal `Courses` proprietà di navigazione del `Department` entità.</span><span class="sxs-lookup"><span data-stu-id="35287-162">To do this, you'll use a nested `GridView` control and databind it to data from the `Courses` navigation property of the `Department` entity.</span></span>

<span data-ttu-id="35287-163">Aprire *Departments.aspx* e nel markup per il `GridView` controllare, specificare un gestore per il `RowDataBound` evento.</span><span class="sxs-lookup"><span data-stu-id="35287-163">Open *Departments.aspx* and in the markup for the `GridView` control, specify a handler for the `RowDataBound` event.</span></span> <span data-ttu-id="35287-164">Il markup per il tag di apertura del controllo è ora simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="35287-164">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

<span data-ttu-id="35287-165">Aggiungere un nuovo `TemplateField` elemento dopo il `Administrator` campo modello:</span><span class="sxs-lookup"><span data-stu-id="35287-165">Add a new `TemplateField` element after the `Administrator` template field:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

<span data-ttu-id="35287-166">Questo codice crea un nidificata `GridView` controllo che mostra il numero di corso e il titolo di un elenco dei corsi.</span><span class="sxs-lookup"><span data-stu-id="35287-166">This markup creates a nested `GridView` control that shows the course number and title of a list of courses.</span></span> <span data-ttu-id="35287-167">Non specifica un'origine dati perché si imposterà un metodo databind all'interno del codice nel `RowDataBound` gestore.</span><span class="sxs-lookup"><span data-stu-id="35287-167">It does not specify a data source because you'll databind it in code in the `RowDataBound` handler.</span></span>

<span data-ttu-id="35287-168">Aprire *Departments.aspx.cs* e aggiungere il gestore seguente per il `RowDataBound` evento:</span><span class="sxs-lookup"><span data-stu-id="35287-168">Open *Departments.aspx.cs* and add the following handler for the `RowDataBound` event:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

<span data-ttu-id="35287-169">Questo codice ottiene la `Department` entità dagli argomenti dell'evento, converte il `Courses` proprietà di navigazione a un `List` raccolta e associa annidata `GridView` alla raccolta.</span><span class="sxs-lookup"><span data-stu-id="35287-169">This code gets the `Department` entity from the event arguments, converts the `Courses` navigation property to a `List` collection, and databinds the nested `GridView` to the collection.</span></span>

<span data-ttu-id="35287-170">Aprire il *SchoolRepository.cs* file e specificare il caricamento eager per le `Courses` proprietà di navigazione chiamando il `Include` metodo della query di oggetto creato nel `GetDepartmentsByName` (metodo).</span><span class="sxs-lookup"><span data-stu-id="35287-170">Open the *SchoolRepository.cs* file and specify eager loading for the `Courses` navigation property by calling the `Include` method in the object query that you create in the `GetDepartmentsByName` method.</span></span> <span data-ttu-id="35287-171">Il `return` istruzione il `GetDepartmentsByName` metodo è ora simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="35287-171">The `return` statement in the `GetDepartmentsByName` method now resembles the following example.</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

<span data-ttu-id="35287-172">Eseguire la pagina.</span><span class="sxs-lookup"><span data-stu-id="35287-172">Run the page.</span></span> <span data-ttu-id="35287-173">Oltre a ordinamento e filtro funzionalità aggiunta in precedenza, il controllo GridView ora mostra i dettagli di corso annidato per ogni reparto.</span><span class="sxs-lookup"><span data-stu-id="35287-173">In addition to the sorting and filtering capability that you added earlier, the GridView control now shows nested course details for each department.</span></span>

<span data-ttu-id="35287-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="35287-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span></span>

<span data-ttu-id="35287-175">In questo passaggio si completa l'introduzione agli scenari di ordinamento, filtro e master-dettagli.</span><span class="sxs-lookup"><span data-stu-id="35287-175">This completes the introduction to sorting, filtering, and master-detail scenarios.</span></span> <span data-ttu-id="35287-176">Nella prossima esercitazione, verrà illustrato come gestire la concorrenza.</span><span class="sxs-lookup"><span data-stu-id="35287-176">In the next tutorial, you'll see how to handle concurrency.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="35287-177">[Precedente](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Successivo](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="35287-177">[Previous](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span></span>
