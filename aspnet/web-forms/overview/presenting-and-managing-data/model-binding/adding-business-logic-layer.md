---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Livello di logica di business aggiunta a un progetto che usa l'associazione di modelli e web form | Microsoft Docs
author: tfitzmac
description: Questa serie di esercitazioni illustra aspetti di base dell'uso di associazione di modelli con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più linee rette-...
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 70e7ae6ad12c26ea5001ff68b25c0431a7440a77
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837873"
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="b9975-104">Livello di logica di business aggiunta a un progetto che usa l'associazione di modelli e web form</span><span class="sxs-lookup"><span data-stu-id="b9975-104">Adding business logic layer to a project that uses model binding and web forms</span></span>
====================
<span data-ttu-id="b9975-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b9975-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b9975-106">Questa serie di esercitazioni illustra aspetti di base dell'uso di associazione di modelli con un progetto di Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b9975-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="b9975-107">Associazione di modelli consente l'interazione dei dati più semplice rispetto a gestione dati di oggetti di origine (ad esempio ObjectDataSource o SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="b9975-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="b9975-108">Questa serie inizia con materiale introduttivo e sposta i concetti più avanzati nelle esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="b9975-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="b9975-109">Questa esercitazione illustra come usare l'associazione di modelli con un livello di logica di business.</span><span class="sxs-lookup"><span data-stu-id="b9975-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="b9975-110">Si imposterà il membro OnCallingDataMethods per specificare che un oggetto diverso dalla pagina corrente viene usato per chiamare i metodi di dati.</span><span class="sxs-lookup"><span data-stu-id="b9975-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="b9975-111">Questa esercitazione si basa sul progetto creato nel [precedenti](retrieving-data.md) parti della serie.</span><span class="sxs-lookup"><span data-stu-id="b9975-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="b9975-112">È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in c# o VB.</span><span class="sxs-lookup"><span data-stu-id="b9975-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="b9975-113">Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="b9975-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="b9975-114">Usa il modello di Visual Studio 2012, che è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="b9975-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="b9975-115">Scopo dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="b9975-115">What you'll build</span></span>

<span data-ttu-id="b9975-116">Associazione di modelli consente di inserire il codice dell'interazione tra i dati nel file code-behind per una pagina web o in una classe per la logica di business separato.</span><span class="sxs-lookup"><span data-stu-id="b9975-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="b9975-117">Nelle esercitazioni precedenti hanno illustrato come utilizzare i file code-behind per il codice di interazione dei dati.</span><span class="sxs-lookup"><span data-stu-id="b9975-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="b9975-118">Questo approccio funziona per i siti di piccole dimensioni, ma può generare codice di ripetizione e maggiore difficoltà durante la gestione di un sito di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="b9975-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="b9975-119">Può anche essere molto difficile testare il codice che si trova nel code-behind file poiché non esiste alcun livello di astrazione a livello di codice.</span><span class="sxs-lookup"><span data-stu-id="b9975-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="b9975-120">Per centralizzare il codice di interazione dei dati, è possibile creare un livello di logica di business che contiene tutta la logica per l'interazione con i dati.</span><span class="sxs-lookup"><span data-stu-id="b9975-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="b9975-121">È quindi possibile chiamare il livello di logica di business dalle pagine web.</span><span class="sxs-lookup"><span data-stu-id="b9975-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="b9975-122">Questa esercitazione illustra come spostare tutto il codice che state scritte nelle esercitazioni precedenti in un livello di logica di business e quindi usare tale codice dalle pagine.</span><span class="sxs-lookup"><span data-stu-id="b9975-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="b9975-123">In questa esercitazione, sarà:</span><span class="sxs-lookup"><span data-stu-id="b9975-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="b9975-124">Spostare il codice da file code-behind a un livello di logica di business</span><span class="sxs-lookup"><span data-stu-id="b9975-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="b9975-125">Modificare i controlli associati a dati per chiamare i metodi nel livello di logica di business</span><span class="sxs-lookup"><span data-stu-id="b9975-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="b9975-126">Creare il livello di logica di business</span><span class="sxs-lookup"><span data-stu-id="b9975-126">Create business logic layer</span></span>

<span data-ttu-id="b9975-127">A questo punto, si creerà la classe che viene chiamata dalle pagine web.</span><span class="sxs-lookup"><span data-stu-id="b9975-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="b9975-128">I metodi in questa classe è un aspetto simili ai metodi che nelle esercitazioni precedenti è stato utilizzato e includono gli attributi di provider di valore.</span><span class="sxs-lookup"><span data-stu-id="b9975-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="b9975-129">In primo luogo, aggiungere una nuova cartella denominata **BLL**.</span><span class="sxs-lookup"><span data-stu-id="b9975-129">First, add a new folder called **BLL**.</span></span>

![Aggiungi cartella](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="b9975-131">Nella cartella di livello BLL, creare una nuova classe denominata **SchoolBL.cs**.</span><span class="sxs-lookup"><span data-stu-id="b9975-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="b9975-132">Conterrà tutte le operazioni di dati che si trovavano originariamente nel file code-behind.</span><span class="sxs-lookup"><span data-stu-id="b9975-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="b9975-133">I metodi sono quasi identici, ai metodi nel file code-behind, ma includeranno alcune modifiche.</span><span class="sxs-lookup"><span data-stu-id="b9975-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="b9975-134">La modifica più importante da notare è che non si eseguono il codice dall'interno di un'istanza di **pagina** classe.</span><span class="sxs-lookup"><span data-stu-id="b9975-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="b9975-135">La classe di pagina contiene le **TryUpdateModel** metodo e il **ModelState** proprietà.</span><span class="sxs-lookup"><span data-stu-id="b9975-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="b9975-136">Quando questo codice viene spostato in un livello di logica di business, non è più necessario un'istanza della classe della pagina per chiamare questi membri.</span><span class="sxs-lookup"><span data-stu-id="b9975-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="b9975-137">Per risolvere questo problema, è necessario aggiungere un **ModelMethodContext** parametro a qualsiasi metodo che accede a ModelState o TryUpdateModel.</span><span class="sxs-lookup"><span data-stu-id="b9975-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="b9975-138">Usare questo parametro ModelMethodContext chiamare TryUpdateModel o recuperare ModelState.</span><span class="sxs-lookup"><span data-stu-id="b9975-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="b9975-139">Non devi modificare nulla nella pagina web per conto di questo nuovo parametro.</span><span class="sxs-lookup"><span data-stu-id="b9975-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="b9975-140">Sostituire il codice in SchoolBL.cs con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="b9975-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="b9975-141">Rivedere le pagine esistenti per recuperare i dati dal livello di logica di business</span><span class="sxs-lookup"><span data-stu-id="b9975-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="b9975-142">Infine, si convertirà le pagine Students.aspx AddStudent.aspx e Courses.aspx rispetto all'uso di query nel file code-behind a utilizzando il livello di logica di business.</span><span class="sxs-lookup"><span data-stu-id="b9975-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="b9975-143">Nel file code-behind per studenti, AddStudent e corsi, eliminare o impostare come commento i metodi di query seguenti:</span><span class="sxs-lookup"><span data-stu-id="b9975-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="b9975-144">studentsGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="b9975-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="b9975-145">studentsGrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="b9975-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="b9975-146">studentsGrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="b9975-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="b9975-147">addStudentForm\_InsertItem</span><span class="sxs-lookup"><span data-stu-id="b9975-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="b9975-148">coursesGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="b9975-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="b9975-149">È ora non necessario alcun codice nel file code-behind che si riferisce alle operazioni sui dati.</span><span class="sxs-lookup"><span data-stu-id="b9975-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="b9975-150">Il **OnCallingDataMethods** gestore eventi consente di specificare un oggetto da utilizzare per i metodi di dati.</span><span class="sxs-lookup"><span data-stu-id="b9975-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="b9975-151">In Students.aspx, aggiungere un valore per tale gestore dell'evento e modificare i nomi dei metodi dei dati per i nomi dei metodi nella classe per la logica di business.</span><span class="sxs-lookup"><span data-stu-id="b9975-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="b9975-152">Nel file code-behind per Students.aspx, definire il gestore eventi per l'evento CallingDataMethods.</span><span class="sxs-lookup"><span data-stu-id="b9975-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="b9975-153">In questo gestore eventi, specificare la classe per la logica di business per le operazioni sui dati.</span><span class="sxs-lookup"><span data-stu-id="b9975-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="b9975-154">In AddStudent.aspx, apportare modifiche analoghe.</span><span class="sxs-lookup"><span data-stu-id="b9975-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="b9975-155">In Courses.aspx, apportare modifiche analoghe.</span><span class="sxs-lookup"><span data-stu-id="b9975-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="b9975-156">Eseguire l'applicazione e si noti che tutte le pagine di funzioni che avevano in precedenza.</span><span class="sxs-lookup"><span data-stu-id="b9975-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="b9975-157">La logica di convalida funziona inoltre correttamente.</span><span class="sxs-lookup"><span data-stu-id="b9975-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="b9975-158">Conclusione</span><span class="sxs-lookup"><span data-stu-id="b9975-158">Conclusion</span></span>

<span data-ttu-id="b9975-159">In questa esercitazione, è strutturata nuovamente l'applicazione per usare un livello di accesso ai dati e il livello di logica di business.</span><span class="sxs-lookup"><span data-stu-id="b9975-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="b9975-160">È stato specificato che i controlli dei dati usano un oggetto che non è la pagina corrente per le operazioni sui dati.</span><span class="sxs-lookup"><span data-stu-id="b9975-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b9975-161">Precedente</span><span class="sxs-lookup"><span data-stu-id="b9975-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
