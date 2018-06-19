---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Livello di logica di business aggiunta a un progetto che utilizza l'associazione di modelli e web form | Documenti Microsoft
author: tfitzmac
description: Questa serie di esercitazioni illustra gli aspetti di base dell'utilizzo di associazione del modello con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 25e887bdc316abf65c780bb6c8d075e938e85064
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892751"
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="f3fb3-104">Livello di logica di business aggiunta a un progetto che utilizza l'associazione di modelli e web form</span><span class="sxs-lookup"><span data-stu-id="f3fb3-104">Adding business logic layer to a project that uses model binding and web forms</span></span>
====================
<span data-ttu-id="f3fb3-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f3fb3-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f3fb3-106">Questa serie di esercitazioni illustra gli aspetti di base dell'utilizzo di associazione del modello con un progetto di Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="f3fb3-107">Associazione di modelli consente l'interazione dei dati più semplice rispetto a gestione dati di oggetti di origine (ad esempio ObjectDataSource o SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="f3fb3-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="f3fb3-108">Questa serie inizia con materiale introduttivo e sposta concetti più avanzati nelle esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="f3fb3-109">In questa esercitazione viene illustrato come utilizzare l'associazione di modelli con un livello di logica di business.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="f3fb3-110">Impostare il membro OnCallingDataMethods per specificare che un oggetto diverso dalla pagina corrente viene utilizzato per chiamare i metodi di dati.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="f3fb3-111">Questa esercitazione si basa sul progetto creato nel [precedenti](retrieving-data.md) parti della serie.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="f3fb3-112">È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in c# o VB.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="f3fb3-113">Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="f3fb3-114">Usa il modello di Visual Studio 2012, è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="f3fb3-115">Occorre invece compilare</span><span class="sxs-lookup"><span data-stu-id="f3fb3-115">What you'll build</span></span>

<span data-ttu-id="f3fb3-116">Associazione di modelli consente di inserire il codice di interazione dei dati in entrambi i file code-behind per una pagina web o in una classe della logica di business separato.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="f3fb3-117">Nelle esercitazioni precedenti hanno illustrato come utilizzare i file code-behind per il codice di interazione dei dati.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="f3fb3-118">Questo approccio funziona per i siti di piccole dimensioni, ma può causare il codice di ripetizione e maggiore difficoltà per la gestione dei siti di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="f3fb3-119">Può anche essere molto difficile da test a livello di programmazione del codice che si trova nel file di codice sottostante perché non esiste alcun livello di astrazione.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="f3fb3-120">Per centralizzare il codice di interazione dei dati, è possibile creare un livello di logica di business che contiene la logica per l'interazione con i dati.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="f3fb3-121">Chiamare quindi il livello di logica di business dalle pagine web.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="f3fb3-122">In questa esercitazione viene illustrato come spostare tutto il codice che scritto nelle esercitazioni precedenti in un livello di logica di business e quindi utilizzare tale codice dalle pagine.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="f3fb3-123">In questa esercitazione, effettuare le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="f3fb3-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="f3fb3-124">Spostare il codice dal file code-behind per un livello di logica di business</span><span class="sxs-lookup"><span data-stu-id="f3fb3-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="f3fb3-125">Modificare i controlli con associazione a dati per chiamare i metodi nel livello di logica di business</span><span class="sxs-lookup"><span data-stu-id="f3fb3-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="f3fb3-126">Creare il livello di logica di business</span><span class="sxs-lookup"><span data-stu-id="f3fb3-126">Create business logic layer</span></span>

<span data-ttu-id="f3fb3-127">A questo punto, si creerà la classe che viene chiamata dalle pagine web.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="f3fb3-128">I metodi in questa classe simile ai metodi utilizzati nelle esercitazioni precedenti e includono gli attributi di provider di valore.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="f3fb3-129">Aggiungere innanzitutto una nuova cartella denominata **BLL**.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-129">First, add a new folder called **BLL**.</span></span>

![aggiungere la cartella](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="f3fb3-131">Nella cartella BLL, creare una nuova classe denominata **SchoolBL.cs**.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="f3fb3-132">Contiene tutte le operazioni di dati che si trovavano originariamente nel file code-behind.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="f3fb3-133">I metodi sono quasi identici, ai metodi nel file code-behind, ma includeranno alcune modifiche.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="f3fb3-134">La modifica più importante da notare è che sono non più in esecuzione il codice all'interno di un'istanza di **pagina** classe.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="f3fb3-135">Contiene la classe delle pagine di **TryUpdateModel** (metodo) e **ModelState** proprietà.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="f3fb3-136">Quando questo codice viene spostato in un livello di logica di business, non è un'istanza della classe di pagina per chiamare i membri.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="f3fb3-137">Per evitare questo problema, è necessario aggiungere un **ModelMethodContext** parametro a un metodo che accede a TryUpdateModel o ModelState.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="f3fb3-138">Utilizzare questo parametro ModelMethodContext per chiamare TryUpdateModel o recuperare ModelState.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="f3fb3-139">Non è necessario modificare qualsiasi elemento nella pagina web per conto di questo nuovo parametro.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="f3fb3-140">Sostituire il codice in SchoolBL.cs con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="f3fb3-141">Rivedere le pagine esistenti per recuperare i dati dal livello di logica di business</span><span class="sxs-lookup"><span data-stu-id="f3fb3-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="f3fb3-142">Infine, si convertirà pagine Students.aspx AddStudent.aspx e Courses.aspx dall'utilizzo di query nel file code-behind per utilizzando il livello di logica di business.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="f3fb3-143">Nel file code-behind per studenti e AddStudent corsi, eliminare o impostare come commento i metodi di query seguenti:</span><span class="sxs-lookup"><span data-stu-id="f3fb3-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="f3fb3-144">studentsGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="f3fb3-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="f3fb3-145">studentsGrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="f3fb3-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="f3fb3-146">studentsGrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="f3fb3-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="f3fb3-147">addStudentForm\_InsertItem</span><span class="sxs-lookup"><span data-stu-id="f3fb3-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="f3fb3-148">coursesGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="f3fb3-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="f3fb3-149">È ora non necessario alcun codice nel file di codice relativo alle operazioni sui dati.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="f3fb3-150">Il **OnCallingDataMethods** gestore eventi consente di specificare un oggetto da utilizzare per i metodi di dati.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="f3fb3-151">In Students.aspx, aggiungere un valore per tale gestore eventi e modificare i nomi dei metodi dei dati per i nomi dei metodi nella classe della logica di business.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="f3fb3-152">Nel file code-behind per Students.aspx, definire il gestore eventi per l'evento CallingDataMethods.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="f3fb3-153">In questo gestore eventi, specificare la classe di logica di business per le operazioni sui dati.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="f3fb3-154">In AddStudent.aspx, apportare modifiche analoghe.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="f3fb3-155">In Courses.aspx, apportare modifiche analoghe.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="f3fb3-156">Eseguire l'applicazione e si noti che tutte le pagine di funzioni che avevano in precedenza.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="f3fb3-157">La logica di convalida funziona inoltre correttamente.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="f3fb3-158">Conclusione</span><span class="sxs-lookup"><span data-stu-id="f3fb3-158">Conclusion</span></span>

<span data-ttu-id="f3fb3-159">In questa esercitazione è strutturato nuovamente l'applicazione di utilizzare un livello di accesso ai dati e il livello di logica di business.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="f3fb3-160">È stato specificato che i controlli di dati utilizzano un oggetto che non è la pagina corrente per le operazioni sui dati.</span><span class="sxs-lookup"><span data-stu-id="f3fb3-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f3fb3-161">Precedente</span><span class="sxs-lookup"><span data-stu-id="f3fb3-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
