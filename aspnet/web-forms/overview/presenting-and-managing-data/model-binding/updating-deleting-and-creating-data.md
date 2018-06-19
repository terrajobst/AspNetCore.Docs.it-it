---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: L'aggiornamento, eliminazione e creazione di dati di associazione del modello e web form | Documenti Microsoft
author: tfitzmac
description: Questa serie di esercitazioni illustra gli aspetti di base dell'utilizzo di associazione del modello con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: e6536f7858afde5faf3aedd34f3cbe95c5ed0d53
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885845"
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a><span data-ttu-id="029fe-104">L'aggiornamento, eliminazione e creazione di dati di associazione del modello e web form</span><span class="sxs-lookup"><span data-stu-id="029fe-104">Updating, deleting, and creating data with model binding and web forms</span></span>
====================
<span data-ttu-id="029fe-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="029fe-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="029fe-106">Questa serie di esercitazioni illustra gli aspetti di base dell'utilizzo di associazione del modello con un progetto di Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="029fe-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="029fe-107">Associazione di modelli consente l'interazione dei dati più semplice rispetto a gestione dati di oggetti di origine (ad esempio ObjectDataSource o SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="029fe-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="029fe-108">Questa serie inizia con materiale introduttivo e sposta concetti più avanzati nelle esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="029fe-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="029fe-109">In questa esercitazione viene illustrato come creare, aggiornare ed eliminare i dati con l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="029fe-109">This tutorial shows how to create, update, and delete data with model binding.</span></span> <span data-ttu-id="029fe-110">Impostare le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="029fe-110">You will set the following properties:</span></span>
> 
> - <span data-ttu-id="029fe-111">DeleteMethod</span><span class="sxs-lookup"><span data-stu-id="029fe-111">DeleteMethod</span></span>
> - <span data-ttu-id="029fe-112">InsertMethod</span><span class="sxs-lookup"><span data-stu-id="029fe-112">InsertMethod</span></span>
> - <span data-ttu-id="029fe-113">UpdateMethod</span><span class="sxs-lookup"><span data-stu-id="029fe-113">UpdateMethod</span></span>
> 
> <span data-ttu-id="029fe-114">Queste proprietà è visualizzato il nome del metodo che gestisce l'operazione corrispondente.</span><span class="sxs-lookup"><span data-stu-id="029fe-114">These properties receive the name of the method that handles the corresponding operation.</span></span> <span data-ttu-id="029fe-115">All'interno di tale metodo, fornire la logica per l'interazione con i dati.</span><span class="sxs-lookup"><span data-stu-id="029fe-115">Within that method, you provide the logic for interacting with the data.</span></span>
> 
> <span data-ttu-id="029fe-116">Questa esercitazione si basa sul progetto creato nel primo [parte](retrieving-data.md) della serie.</span><span class="sxs-lookup"><span data-stu-id="029fe-116">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="029fe-117">È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in c# o VB.</span><span class="sxs-lookup"><span data-stu-id="029fe-117">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="029fe-118">Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="029fe-118">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="029fe-119">Usa il modello di Visual Studio 2012, è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="029fe-119">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="029fe-120">Occorre invece compilare</span><span class="sxs-lookup"><span data-stu-id="029fe-120">What you'll build</span></span>

<span data-ttu-id="029fe-121">In questa esercitazione, effettuare le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="029fe-121">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="029fe-122">Aggiungere modelli di dati dinamici</span><span class="sxs-lookup"><span data-stu-id="029fe-122">Add dynamic data templates</span></span>
2. <span data-ttu-id="029fe-123">Abilitare l'aggiornamento ed eliminazione dei dati tramite i metodi di associazione del modello</span><span class="sxs-lookup"><span data-stu-id="029fe-123">Enable updating and deleting data through model binding methods</span></span>
3. <span data-ttu-id="029fe-124">Applicare le regole di convalida di dati: consentono di creare un nuovo record nel database</span><span class="sxs-lookup"><span data-stu-id="029fe-124">Apply data validation rules - Enable creating a new record in the database</span></span>

## <a name="add-dynamic-data-templates"></a><span data-ttu-id="029fe-125">Aggiungere modelli di dati dinamici</span><span class="sxs-lookup"><span data-stu-id="029fe-125">Add dynamic data templates</span></span>

<span data-ttu-id="029fe-126">Per offrire la migliore esperienza utente e ridurre al minimo ripetizioni nel codice, si utilizzeranno i modelli di dati dinamica.</span><span class="sxs-lookup"><span data-stu-id="029fe-126">To provide the best user experience and minimize code repetition, you will use dynamic data templates.</span></span> <span data-ttu-id="029fe-127">È possibile integrare facilmente i modelli di dati dinamica preesistente nel sito esistente mediante l'installazione di un pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="029fe-127">You can easily integrate pre-built dynamic data templates into your existing site by installing a NuGet package.</span></span>

<span data-ttu-id="029fe-128">Dal **Gestisci pacchetti NuGet**, installare il **DynamicDataTemplatesCS**.</span><span class="sxs-lookup"><span data-stu-id="029fe-128">From the **Manage NuGet Packages**, install the **DynamicDataTemplatesCS**.</span></span>

![modelli di dati dinamici](updating-deleting-and-creating-data/_static/image1.png)

<span data-ttu-id="029fe-130">Si noti che il progetto include ora una cartella denominata **DynamicData**.</span><span class="sxs-lookup"><span data-stu-id="029fe-130">Notice that your project now includes a folder named **DynamicData**.</span></span> <span data-ttu-id="029fe-131">In questa cartella sono disponibili i modelli vengono applicati automaticamente ai controlli dinamici nei moduli web.</span><span class="sxs-lookup"><span data-stu-id="029fe-131">In that folder, you will find the templates that are automatically applied to dynamic controls in your web forms.</span></span>

![cartella Dynamic data](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a><span data-ttu-id="029fe-133">Abilita aggiornamento ed eliminazione</span><span class="sxs-lookup"><span data-stu-id="029fe-133">Enable updating and deleting</span></span>

<span data-ttu-id="029fe-134">Consentire agli utenti di aggiornare ed eliminare i record nel database è molto simile al processo per il recupero dei dati.</span><span class="sxs-lookup"><span data-stu-id="029fe-134">Enabling users to update and delete records in the database is very similar to the process for retrieving data.</span></span> <span data-ttu-id="029fe-135">Nel **UpdateMethod** e **DeleteMethod** proprietà, specificare i nomi dei metodi che eseguono tali operazioni.</span><span class="sxs-lookup"><span data-stu-id="029fe-135">In the **UpdateMethod** and **DeleteMethod** properties, you specify the names of the methods that perform those operations.</span></span> <span data-ttu-id="029fe-136">Un controllo GridView, è anche possibile specificare la generazione automatica di modifica ed eliminare i pulsanti.</span><span class="sxs-lookup"><span data-stu-id="029fe-136">With a GridView control, you can also specify the automatic generation of edit and delete buttons.</span></span> <span data-ttu-id="029fe-137">Il codice evidenziato di seguito mostra le aggiunte al codice GridView.</span><span class="sxs-lookup"><span data-stu-id="029fe-137">The following highlighted code shows the additions to the GridView code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

<span data-ttu-id="029fe-138">Nel file code-behind, aggiungere un tramite l'istruzione per **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="029fe-138">In the code-behind file, add a using statement for **System.Data.Entity.Infrastructure**.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

<span data-ttu-id="029fe-139">Quindi, aggiungere il seguente aggiornamento ed eliminazione di metodi.</span><span class="sxs-lookup"><span data-stu-id="029fe-139">Then, add the following update and delete methods.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

<span data-ttu-id="029fe-140">Il **TryUpdateModel** metodo si applica l'associazione a dati i valori corrispondenti dal web form per l'elemento di dati.</span><span class="sxs-lookup"><span data-stu-id="029fe-140">The **TryUpdateModel** method applies the matching data-bound values from the web form to the data item.</span></span> <span data-ttu-id="029fe-141">L'elemento di dati verrà recuperato in base al valore del parametro id.</span><span class="sxs-lookup"><span data-stu-id="029fe-141">The data item is retrieved based on the value of the id parameter.</span></span>

## <a name="enforce-validation-requirements"></a><span data-ttu-id="029fe-142">Applicare i requisiti di convalida</span><span class="sxs-lookup"><span data-stu-id="029fe-142">Enforce validation requirements</span></span>

<span data-ttu-id="029fe-143">Gli attributi di convalida che applica alle proprietà FirstName, LastName e l'anno nella classe Student vengono applicati automaticamente quando l'aggiornamento dei dati.</span><span class="sxs-lookup"><span data-stu-id="029fe-143">The validation attributes that you applied to the FirstName, LastName, and Year properties in the Student class are automatically enforced when updating the data.</span></span> <span data-ttu-id="029fe-144">I controlli di DynamicField aggiungono validator client e server in base agli attributi di convalida.</span><span class="sxs-lookup"><span data-stu-id="029fe-144">The DynamicField controls add client and server validators based on the validation attributes.</span></span> <span data-ttu-id="029fe-145">Le proprietà FirstName e LastName sono entrambi necessari.</span><span class="sxs-lookup"><span data-stu-id="029fe-145">The FirstName and LastName properties are both required.</span></span> <span data-ttu-id="029fe-146">Non può superare i 20 caratteri FirstName e LastName non può superare i 40 caratteri.</span><span class="sxs-lookup"><span data-stu-id="029fe-146">FirstName cannot exceed 20 characters in length, and LastName cannot exceed 40 characters.</span></span> <span data-ttu-id="029fe-147">Anno deve essere un valore valido per l'enumerazione AcademicYear.</span><span class="sxs-lookup"><span data-stu-id="029fe-147">Year must be a valid value for the AcademicYear enumeration.</span></span>

<span data-ttu-id="029fe-148">Se l'utente viola uno dei requisiti di convalida, l'aggiornamento non verrà eseguito.</span><span class="sxs-lookup"><span data-stu-id="029fe-148">If the user violates one of the validation requirements, the update does not proceed.</span></span> <span data-ttu-id="029fe-149">Per visualizzare il messaggio di errore, aggiungere un controllo ValidationSummary sopra il controllo GridView.</span><span class="sxs-lookup"><span data-stu-id="029fe-149">To see the error message, add a ValidationSummary control above the GridView.</span></span> <span data-ttu-id="029fe-150">Per visualizzare gli errori di convalida dell'associazione modello, impostare il **ShowModelStateErrors** proprietà impostata su **true**.</span><span class="sxs-lookup"><span data-stu-id="029fe-150">To display the validation errors from model binding, set the **ShowModelStateErrors** property set to **true**.</span></span> 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

<span data-ttu-id="029fe-151">Eseguire l'applicazione web e aggiornare ed eliminare tutti i record.</span><span class="sxs-lookup"><span data-stu-id="029fe-151">Run the web application, and update and delete any of the records.</span></span>

![Aggiornare i dati](updating-deleting-and-creating-data/_static/image3.png)

<span data-ttu-id="029fe-153">Si noti che quando la modalità di modifica il valore per la proprietà Year viene automaticamente eseguito il rendering come un elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="029fe-153">Notice that when in the edit mode the value for the Year property is automatically rendered as a drop down list.</span></span> <span data-ttu-id="029fe-154">La proprietà Year è un valore di enumerazione e il modello di dati dinamica per un valore di enumerazione specifica un menu a discesa per la modifica.</span><span class="sxs-lookup"><span data-stu-id="029fe-154">The Year property is an enumeration value, and the dynamic data template for an enumeration value specifies a drop down list for editing.</span></span> <span data-ttu-id="029fe-155">È possibile trovare tale modello aprendo il **enumerazione\_Edit. ascx** file nel **DynamicData**/**FieldTemplates** cartella.</span><span class="sxs-lookup"><span data-stu-id="029fe-155">You can find that template by opening the **Enumeration\_Edit.ascx** file in the **DynamicData**/**FieldTemplates** folder.</span></span>

<span data-ttu-id="029fe-156">Se si forniscono i valori validi, l'aggiornamento viene completato correttamente.</span><span class="sxs-lookup"><span data-stu-id="029fe-156">If you provide valid values, the update completes successfully.</span></span> <span data-ttu-id="029fe-157">In caso di violazione di uno dei requisiti di convalida, l'aggiornamento non verrà eseguito e un messaggio di errore viene visualizzato sopra la griglia.</span><span class="sxs-lookup"><span data-stu-id="029fe-157">If you violate one of the validation requirements, the update does not proceed and an error message is displayed above the grid.</span></span>

![Messaggio di errore](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a><span data-ttu-id="029fe-159">Aggiunta di nuovi record</span><span class="sxs-lookup"><span data-stu-id="029fe-159">Add new records</span></span>

<span data-ttu-id="029fe-160">Il controllo GridView non include il **InsertMethod** proprietà e pertanto non può essere utilizzato per l'aggiunta di un nuovo record con l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="029fe-160">The GridView control does not include the **InsertMethod** property and therefore cannot be used for adding a new record with model binding.</span></span> <span data-ttu-id="029fe-161">È possibile trovare la proprietà InsertMethod nel **FormView**, **DetailsView**, o **ListView** controlli.</span><span class="sxs-lookup"><span data-stu-id="029fe-161">You can find the InsertMethod property in the **FormView**, **DetailsView**, or **ListView** controls.</span></span> <span data-ttu-id="029fe-162">In questa esercitazione si utilizzerà un controllo FormView per aggiungere un nuovo record.</span><span class="sxs-lookup"><span data-stu-id="029fe-162">In this tutorial, you will use a FormView control to add a new record.</span></span>

<span data-ttu-id="029fe-163">In primo luogo, aggiungere un collegamento alla nuova pagina che viene creata per l'aggiunta di un nuovo record.</span><span class="sxs-lookup"><span data-stu-id="029fe-163">First, add a link to the new page you will create for adding a new record.</span></span> <span data-ttu-id="029fe-164">Sopra il controllo ValidationSummary, aggiungere:</span><span class="sxs-lookup"><span data-stu-id="029fe-164">Above the ValidationSummary, add:</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

<span data-ttu-id="029fe-165">Il nuovo collegamento verrà visualizzato nella parte superiore del contenuto per la pagina di studenti.</span><span class="sxs-lookup"><span data-stu-id="029fe-165">The new link will appear at the top of the content for the Students page.</span></span>

![nuovo collegamento](updating-deleting-and-creating-data/_static/image5.png)

<span data-ttu-id="029fe-167">Quindi, aggiungere un nuovo web form mediante pagina master e denominarlo **AddStudent**.</span><span class="sxs-lookup"><span data-stu-id="029fe-167">Then, add a new web form using a master page, and name it **AddStudent**.</span></span> <span data-ttu-id="029fe-168">Selezionare la pagina master Site. master.</span><span class="sxs-lookup"><span data-stu-id="029fe-168">Select Site.Master as the master page.</span></span>

<span data-ttu-id="029fe-169">Verrà eseguito il rendering dell'oggetto i campi per l'aggiunta di un nuovo studente utilizzando un **DynamicEntity** controllo.</span><span class="sxs-lookup"><span data-stu-id="029fe-169">You will render the fields for adding a new student by using a **DynamicEntity** control.</span></span> <span data-ttu-id="029fe-170">Il controllo DynamicEntity esegue il rendering di tale proprietà modificabili nella classe specificata nella proprietà ItemType.</span><span class="sxs-lookup"><span data-stu-id="029fe-170">The DynamicEntity control renders that editable properties in the class specified in the ItemType property.</span></span> <span data-ttu-id="029fe-171">La proprietà StudentID è stata contrassegnata con il **[ScaffoldColumn(false)]** attributo in modo che non viene eseguito il rendering.</span><span class="sxs-lookup"><span data-stu-id="029fe-171">The StudentID property was marked with the **[ScaffoldColumn(false)]** attribute so it is not rendered.</span></span> <span data-ttu-id="029fe-172">Segnaposto della pagina AddStudent MainContent, aggiungere il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="029fe-172">In the MainContent placeholder of the AddStudent page, add the following code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

<span data-ttu-id="029fe-173">Nel file code-behind (AddStudent.aspx.cs), aggiungere un **utilizzando** istruzione per il **ContosoUniversityModelBinding.Models** dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="029fe-173">In the code-behind file (AddStudent.aspx.cs), add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

<span data-ttu-id="029fe-174">Aggiungere quindi i metodi seguenti per specificare la modalità di inserimento di un nuovo record e un gestore eventi per il pulsante Annulla.</span><span class="sxs-lookup"><span data-stu-id="029fe-174">Then, add the following methods to specify how to insert a new record and an event handler for the cancel button.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

<span data-ttu-id="029fe-175">Salvare tutte le modifiche.</span><span class="sxs-lookup"><span data-stu-id="029fe-175">Save all of the changes.</span></span>

<span data-ttu-id="029fe-176">Eseguire l'applicazione web e creare un nuovo studente.</span><span class="sxs-lookup"><span data-stu-id="029fe-176">Run the web application and create a new student.</span></span>

![Aggiungere nuovo studente](updating-deleting-and-creating-data/_static/image6.png)

<span data-ttu-id="029fe-178">Fare clic su **inserire** e notare che è stato creato il nuovo studente.</span><span class="sxs-lookup"><span data-stu-id="029fe-178">Click **Insert** and notice the new student has been created.</span></span>

![visualizzare di nuovo studente](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a><span data-ttu-id="029fe-180">Conclusione</span><span class="sxs-lookup"><span data-stu-id="029fe-180">Conclusion</span></span>

<span data-ttu-id="029fe-181">In questa esercitazione è abilitato l'aggiornamento, eliminazione e creazione di dati.</span><span class="sxs-lookup"><span data-stu-id="029fe-181">In this tutorial, you enabled updating, deleting, and creating data.</span></span> <span data-ttu-id="029fe-182">Si verificato durante l'interazione con i dati vengono applicate regole di convalida.</span><span class="sxs-lookup"><span data-stu-id="029fe-182">You ensured validation rules are applied when interacting with the data.</span></span>

<span data-ttu-id="029fe-183">Nella prossima [esercitazione](sorting-paging-and-filtering-data.md) in questa serie, si abiliterà ordinamento, paging e filtro dei dati.</span><span class="sxs-lookup"><span data-stu-id="029fe-183">In the next [tutorial](sorting-paging-and-filtering-data.md) in this series, you will enable sorting, paging, and filtering data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="029fe-184">[Precedente](retrieving-data.md)
> [Successivo](sorting-paging-and-filtering-data.md)</span><span class="sxs-lookup"><span data-stu-id="029fe-184">[Previous](retrieving-data.md)
[Next](sorting-paging-and-filtering-data.md)</span></span>
