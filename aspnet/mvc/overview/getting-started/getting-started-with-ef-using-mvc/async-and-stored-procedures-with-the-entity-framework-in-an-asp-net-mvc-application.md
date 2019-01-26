---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Esercitazione: Utilizzare async e stored procedure con Entity Framework in un'App MVC ASP.NET"
description: In questa esercitazione viene illustrato come implementare il modello di programmazione asincrono e informazioni su come usare stored procedure.
author: tdykstra
ms.author: riande
ms.date: 01/18/2019
ms.topic: tutorial
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9041167af076d80ebf294e054ffe51293d11e888
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889873"
---
# <a name="tutorial-use-async-and-stored-procedures-with-ef-in-an-aspnet-mvc-app"></a><span data-ttu-id="6b1f1-103">Esercitazione: Utilizzare async e stored procedure con Entity Framework in un'App MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6b1f1-103">Tutorial: Use async and stored procedures with EF in an ASP.NET MVC App</span></span>

<span data-ttu-id="6b1f1-104">Nelle esercitazioni precedenti è stato descritto come leggere e aggiornare i dati usando il modello di programmazione sincrona.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-104">In earlier tutorials you learned how to read and update data using the synchronous programming model.</span></span> <span data-ttu-id="6b1f1-105">In questa esercitazione viene illustrato come implementare il modello di programmazione asincrono.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-105">In this tutorial you see how to implement the asynchronous programming model.</span></span> <span data-ttu-id="6b1f1-106">Codice asincrono consente a un'applicazione di offrono prestazioni migliori perché rende un migliore utilizzo delle risorse del server.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-106">Asynchronous code can help an application perform better because it makes better use of server resources.</span></span>

<span data-ttu-id="6b1f1-107">In questa esercitazione inoltre illustrato come utilizzare le stored procedure di inserimento, aggiornamento e le operazioni di eliminazione su un'entità.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-107">In this tutorial you also see how to use stored procedures for insert, update, and delete operations on an entity.</span></span>

<span data-ttu-id="6b1f1-108">Infine, si ridistribuisce l'applicazione in Azure, insieme a tutte le modifiche del database che sono stati implementati dopo la prima volta che è stato distribuito.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-108">Finally, you redeploy the application to Azure, along with all of the database changes that you've implemented since the first time you deployed.</span></span>

<span data-ttu-id="6b1f1-109">Le figure seguenti illustrano alcune delle pagine che verranno usate.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-109">The following illustrations show some of the pages that you'll work with.</span></span>

![Pagina Departments](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Creare reparto](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="6b1f1-112">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="6b1f1-112">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6b1f1-113">Informazioni su codice asincrono</span><span class="sxs-lookup"><span data-stu-id="6b1f1-113">Learn about asynchronous code</span></span>
> * <span data-ttu-id="6b1f1-114">Creare un controller di reparto</span><span class="sxs-lookup"><span data-stu-id="6b1f1-114">Create a Department controller</span></span>
> * <span data-ttu-id="6b1f1-115">Utilizzare le stored procedure</span><span class="sxs-lookup"><span data-stu-id="6b1f1-115">Use stored procedures</span></span>
> * <span data-ttu-id="6b1f1-116">Distribuire in Azure</span><span class="sxs-lookup"><span data-stu-id="6b1f1-116">Deploy to Azure</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6b1f1-117">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="6b1f1-117">Prerequisites</span></span>

* [<span data-ttu-id="6b1f1-118">Aggiornamento di dati correlati</span><span class="sxs-lookup"><span data-stu-id="6b1f1-118">Updating Related Data</span></span>](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="why-use-asynchronous-code"></a><span data-ttu-id="6b1f1-119">Perché usare codice asincrono</span><span class="sxs-lookup"><span data-stu-id="6b1f1-119">Why use asynchronous code</span></span>

<span data-ttu-id="6b1f1-120">Per un server Web è disponibile un numero limitato di thread e in situazioni di carico elevato tutti i thread disponibili potrebbero essere in uso.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-120">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="6b1f1-121">In queste circostanze il server non può elaborare nuove richieste finché i thread non saranno liberi.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-121">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="6b1f1-122">Con il codice sincrono, può succedere che molti thread siano vincolati nonostante in quel momento non stiano eseguendo alcuna operazione. Rimangono tuttavia in attesa che l'operazione I/O sia completata.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-122">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="6b1f1-123">Con il codice asincrono, se un processo è in attesa del completamento dell'operazione I/O, il thread viene liberato e il server lo può usare per l'elaborazione di altre richieste.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-123">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="6b1f1-124">Di conseguenza, codice asincrono consente alle risorse di server da usare in modo più efficiente e il server è abilitato per gestire più traffico senza ritardi.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-124">As a result, asynchronous code enables server resources to be use more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="6b1f1-125">Nelle versioni precedenti di .NET, la scrittura e test del codice asincrono era complessa, soggetto a errori e difficile eseguire il debug.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-125">In earlier versions of .NET, writing and testing asynchronous code was complex, error prone, and hard to debug.</span></span> <span data-ttu-id="6b1f1-126">In .NET 4.5, la scrittura di test e debug del codice asincrono è molto più semplice che è consigliabile scrivere codice asincrono a meno che non si abbia un motivo non.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-126">In .NET 4.5, writing, testing, and debugging asynchronous code is so much easier that you should generally write asynchronous code unless you have a reason not to.</span></span> <span data-ttu-id="6b1f1-127">Codice asincrono che presenta una piccola quantità di overhead, ma per le situazioni di traffico ridotto il calo delle prestazioni è irrilevante, mentre per le situazioni di traffico elevato, il potenziale miglioramento delle prestazioni è notevole.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-127">Asynchronous code does introduce a small amount of overhead, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="6b1f1-128">Per altre informazioni sulla programmazione asincrona, vedere [supporto di usare .NET 4.5 asincrono per evitare di bloccare le chiamate](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span><span class="sxs-lookup"><span data-stu-id="6b1f1-128">For more information about asynchronous programming, see [Use .NET 4.5's async support to avoid blocking calls](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span></span>

## <a name="create-department-controller"></a><span data-ttu-id="6b1f1-129">Creare controller Department</span><span class="sxs-lookup"><span data-stu-id="6b1f1-129">Create Department controller</span></span>

<span data-ttu-id="6b1f1-130">Creare un controller Department allo stesso modo è stato eseguito il precedente controller, ma questa volta selezionare il **usare le azioni del controller asincrono** casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-130">Create a Department controller the same way you did the earlier controllers, except this time select the **Use async controller actions** check box.</span></span>

<span data-ttu-id="6b1f1-131">Le caratteristiche principali seguenti illustrano ciò che è stato aggiunto al codice sincrono per il `Index` metodo per renderla asincrona:</span><span class="sxs-lookup"><span data-stu-id="6b1f1-131">The following highlights show what was added to the synchronous code for the `Index` method to make it asynchronous:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

<span data-ttu-id="6b1f1-132">Quattro modifiche sono state applicate per consentire alla query di database di Entity Framework di eseguire in modo asincrono:</span><span class="sxs-lookup"><span data-stu-id="6b1f1-132">Four changes were applied to enable the Entity Framework database query to execute asynchronously:</span></span>

- <span data-ttu-id="6b1f1-133">Il metodo è contrassegnato con il `async` parola chiave, che indica al compilatore di generare callback per parti del corpo del metodo e per la creazione automatica di `Task<ActionResult>` oggetto restituito.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-133">The method is marked with the `async` keyword, which tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<ActionResult>` object that is returned.</span></span>
- <span data-ttu-id="6b1f1-134">Il tipo restituito è stato modificato da `ActionResult` a `Task<ActionResult>`.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-134">The return type was changed from `ActionResult` to `Task<ActionResult>`.</span></span> <span data-ttu-id="6b1f1-135">Il `Task<T>` tipo rappresenta il lavoro in corso con un risultato di tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-135">The `Task<T>` type represents ongoing work with a result of type `T`.</span></span>
- <span data-ttu-id="6b1f1-136">Il `await` è stata applicata la parola chiave per la chiamata al servizio web.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-136">The `await` keyword was applied to the web service call.</span></span> <span data-ttu-id="6b1f1-137">Quando il compilatore rileva questa parola chiave, dietro le quinte suddivide il metodo in due parti.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-137">When the compiler sees this keyword, behind the scenes it splits the method into two parts.</span></span> <span data-ttu-id="6b1f1-138">La prima parte termina con l'operazione che viene avviato in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-138">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="6b1f1-139">La seconda parte viene inserita in un metodo di callback che viene chiamato al termine dell'operazione.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-139">The second part is put into a callback method that is called when the operation completes.</span></span>
- <span data-ttu-id="6b1f1-140">La versione asincrona del `ToList` è stato chiamato il metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-140">The asynchronous version of the `ToList` extension method was called.</span></span>

<span data-ttu-id="6b1f1-141">Il motivo per cui è il `departments.ToList` istruzione modificata ma non la `departments = db.Departments` istruzione?</span><span class="sxs-lookup"><span data-stu-id="6b1f1-141">Why is the `departments.ToList` statement modified but not the `departments = db.Departments` statement?</span></span> <span data-ttu-id="6b1f1-142">Il motivo è che solo le istruzioni che generano query o comandi da inviare al database vengono eseguite in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-142">The reason is that only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="6b1f1-143">Il `departments = db.Departments` istruzione imposta una query, ma finché non viene eseguita la query di `ToList` viene chiamato il metodo.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-143">The `departments = db.Departments` statement sets up a query but the query is not executed until the `ToList` method is called.</span></span> <span data-ttu-id="6b1f1-144">Pertanto, solo il `ToList` metodo viene eseguito in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-144">Therefore, only the `ToList` method is executed asynchronously.</span></span>

<span data-ttu-id="6b1f1-145">Nel `Details` metodo e il `HttpGet` `Edit` e `Delete` metodi, il `Find` metodo è quello che fa sì che una query da inviare al database, in modo che il metodo che viene eseguito in modo asincrono:</span><span class="sxs-lookup"><span data-stu-id="6b1f1-145">In the `Details` method and the `HttpGet` `Edit` and `Delete` methods, the `Find` method is the one that causes a query to be sent to the database, so that's the method that gets executed asynchronously:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

<span data-ttu-id="6b1f1-146">Nel `Create`, `HttpPost Edit`, e `DeleteConfirmed` metodi, è il `SaveChanges` chiamata al metodo che fa sì che un comando da eseguire, non le istruzioni, ad esempio `db.Departments.Add(department)` che causano solo le entità in memoria da modificare.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-146">In the `Create`, `HttpPost Edit`, and `DeleteConfirmed` methods, it is the `SaveChanges` method call that causes a command to be executed, not statements such as `db.Departments.Add(department)` which only cause entities in memory to be modified.</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

<span data-ttu-id="6b1f1-147">Aprire *Views\Department\Index.cshtml*e sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="6b1f1-147">Open *Views\Department\Index.cshtml*, and replace the template code with the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

<span data-ttu-id="6b1f1-148">Questo codice imposta il titolo dall'indice in reparti, sposta il nome dell'amministratore a destra e fornisce il nome completo dell'amministratore.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-148">This code changes the title from Index to Departments, moves the Administrator name to the right, and provides the full name of the administrator.</span></span>

<span data-ttu-id="6b1f1-149">Nella creazione, eliminazione, dettagli e modificare le viste, modificare la didascalia per il `InstructorID` campo su "Administrator" simile a quello è modificato il campo del nome reparto "Department" le visualizzazioni dei corsi.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-149">In the Create, Delete, Details, and Edit views, change the caption for the `InstructorID` field to "Administrator" the same way you changed the department name field to "Department" in the Course views.</span></span>

<span data-ttu-id="6b1f1-150">Nella creazione e modifica le visualizzazioni usano il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="6b1f1-150">In the Create and Edit views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

<span data-ttu-id="6b1f1-151">Nelle visualizzazioni Delete e dettagli usare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="6b1f1-151">In the Delete and Details views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

<span data-ttu-id="6b1f1-152">Eseguire l'applicazione e scegliere il **reparti** scheda.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-152">Run the application, and click the **Departments** tab.</span></span>

<span data-ttu-id="6b1f1-153">Tutto funziona esattamente come in altri controller, ma in questo controller di tutte le query SQL sono in esecuzione in modo asincrono.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-153">Everything works the same as in the other controllers, but in this controller all of the SQL queries are executing asynchronously.</span></span>

<span data-ttu-id="6b1f1-154">Alcuni aspetti da tenere presenti quando si usa la programmazione asincrona con Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="6b1f1-154">Some things to be aware of when you are using asynchronous programming with the Entity Framework:</span></span>

- <span data-ttu-id="6b1f1-155">Il codice asincrono non è thread-safe.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-155">The async code is not thread safe.</span></span> <span data-ttu-id="6b1f1-156">In altre parole, in altre parole, non tentare di eseguire più operazioni in parallelo usando la stessa istanza di contesto.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-156">In other words, in other words, don't try to do multiple operations in parallel using the same context instance.</span></span>
- <span data-ttu-id="6b1f1-157">Se si vogliono sfruttare i vantaggi del codice asincrono in termini di prestazioni, verificare che i pacchetti della libreria impiegati, ad esempio per il paging, usino la modalità asincrona per chiamare i metodi di Entity Framework che generano query da inviare al database.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-157">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

## <a name="use-stored-procedures"></a><span data-ttu-id="6b1f1-158">Utilizzare le stored procedure</span><span class="sxs-lookup"><span data-stu-id="6b1f1-158">Use stored procedures</span></span>

<span data-ttu-id="6b1f1-159">Alcuni sviluppatori e amministratori di database preferiscono usare stored procedure per l'accesso al database.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-159">Some developers and DBAs prefer to use stored procedures for database access.</span></span> <span data-ttu-id="6b1f1-160">Nelle versioni precedenti di Entity Framework è possibile recuperare i dati utilizzando una stored procedure dal [l'esecuzione di una query SQL non elaborata](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), ma è non è possibile indicare a Entity Framework per utilizzare le stored procedure per le operazioni di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-160">In earlier versions of Entity Framework you can retrieve data using a stored procedure by [executing a raw SQL query](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), but you can't instruct EF to use stored procedures for update operations.</span></span> <span data-ttu-id="6b1f1-161">In EF 6 è semplice da configurare Code First per utilizzare le stored procedure.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-161">In EF 6 it's easy to configure Code First to use stored procedures.</span></span>

1. <span data-ttu-id="6b1f1-162">Nelle *DAL\SchoolContext.cs*, aggiungere il codice evidenziato per il `OnModelCreating` (metodo).</span><span class="sxs-lookup"><span data-stu-id="6b1f1-162">In *DAL\SchoolContext.cs*, add the highlighted code to the `OnModelCreating` method.</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    <span data-ttu-id="6b1f1-163">Questo codice indica a Entity Framework per utilizzare stored procedure per l'inserimento, aggiornamento ed eliminazione di operazioni su di `Department` entità.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-163">This code instructs Entity Framework to use stored procedures for insert, update, and delete operations on the `Department` entity.</span></span>
2. <span data-ttu-id="6b1f1-164">Nella Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="6b1f1-164">In Package Manage Console, enter the following command:</span></span>

    `add-migration DepartmentSP`

    <span data-ttu-id="6b1f1-165">Aprire *migrazioni\&lt; timestamp&gt;\_DepartmentSP.cs* per visualizzare il codice nel `Up` metodo che crea Insert, Update e Delete stored procedure:</span><span class="sxs-lookup"><span data-stu-id="6b1f1-165">Open *Migrations\&lt;timestamp&gt;\_DepartmentSP.cs* to see the code in the `Up` method that creates Insert, Update, and Delete stored procedures:</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. <span data-ttu-id="6b1f1-166">Nella Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="6b1f1-166">In Package Manage Console, enter the following command:</span></span>

     `update-database`
4. <span data-ttu-id="6b1f1-167">Esegue l'applicazione in modalità di debug, scegliere il **reparti** scheda e quindi fare clic su **Crea nuovo**.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-167">Run the application in debug mode, click the **Departments** tab, and then click **Create New**.</span></span>
5. <span data-ttu-id="6b1f1-168">Immettere i dati per un nuovo reparto e quindi fare clic su **Create**.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-168">Enter data for a new department, and then click **Create**.</span></span>

6. <span data-ttu-id="6b1f1-169">Esaminare i log in Visual Studio, il **Output** finestra per verificare che una stored procedure è stata usata per inserire la nuova riga di reparto.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-169">In Visual Studio, look at the logs in the **Output** window to see that a stored procedure was used to insert the new Department row.</span></span>

     ![Reparto Insert SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="6b1f1-171">Codice crea prima di tutto i nomi di procedura predefinita memorizzata.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-171">Code First creates default stored procedure names.</span></span> <span data-ttu-id="6b1f1-172">Se si usa un database esistente, si potrebbe essere necessario personalizzare i nomi di stored procedure per utilizzare le stored procedure già definite nel database.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-172">If you are using an existing database, you might need to customize the stored procedure names in order to use stored procedures already defined in the database.</span></span> <span data-ttu-id="6b1f1-173">Per informazioni su come eseguire questa operazione, vedere [Entity Framework Code prima Insert/Update/Delete Stored Procedures](https://msdn.microsoft.com/data/dn468673).</span><span class="sxs-lookup"><span data-stu-id="6b1f1-173">For information about how to do that, see [Entity Framework Code First Insert/Update/Delete Stored Procedures](https://msdn.microsoft.com/data/dn468673).</span></span>

<span data-ttu-id="6b1f1-174">Se si desidera personalizzare cosa generato eseguire stored procedure, è possibile modificare il codice con scaffolding per le migrazioni `Up` metodo che crea la stored procedure.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-174">If you want to customize what generated stored procedures do, you can edit the scaffolded code for the migrations `Up` method that creates the stored procedure.</span></span> <span data-ttu-id="6b1f1-175">In questo modo le modifiche verranno riflesse ogni volta che la migrazione viene eseguita e verrà applicata al database di produzione quando migrazioni viene eseguito automaticamente nell'ambiente di produzione dopo la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-175">That way your changes are reflected whenever that migration is run and will be applied to your production database when migrations runs automatically in production after deployment.</span></span>

<span data-ttu-id="6b1f1-176">Se si desidera modificare una stored procedure esistente che è stata creata in una migrazione precedente, è possibile usare il comando Add-Migration per generare una migrazione vuota e quindi scrivere manualmente il codice che chiama il [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) (metodo) .</span><span class="sxs-lookup"><span data-stu-id="6b1f1-176">If you want to change an existing stored procedure that was created in a previous migration, you can use the Add-Migration command to generate a blank migration, and then manually write code that calls the [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) method.</span></span>

## <a name="deploy-to-azure"></a><span data-ttu-id="6b1f1-177">Distribuire in Azure</span><span class="sxs-lookup"><span data-stu-id="6b1f1-177">Deploy to Azure</span></span>

<span data-ttu-id="6b1f1-178">In questa sezione è necessario aver completato l'opzione facoltativa **la distribuzione dell'app in Azure** sezione il [migrazioni e la distribuzione](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) esercitazione della serie.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-178">This section requires you to have completed the optional **Deploying the app to Azure** section in the [Migrations and Deployment](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial of this series.</span></span> <span data-ttu-id="6b1f1-179">Se si sono verificati errori di migrazione che è stato risolto, eliminare il database nel progetto locale, ignorare questa sezione.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-179">If you had migrations errors that you resolved by deleting the database in your local project, skip this section.</span></span>

1. <span data-ttu-id="6b1f1-180">In Visual Studio, fare clic sul progetto in **Esplora soluzioni** e selezionare **Publish** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-180">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>
2. <span data-ttu-id="6b1f1-181">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-181">Click **Publish**.</span></span>

    <span data-ttu-id="6b1f1-182">Visual Studio distribuisce l'applicazione in Azure e l'applicazione viene aperta nel browser predefinito, in esecuzione in Azure.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-182">Visual Studio deploys the application to Azure, and the application opens in your default browser, running in Azure.</span></span>
3. <span data-ttu-id="6b1f1-183">Testare l'applicazione per verificare funzioni.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-183">Test the application to verify it's working.</span></span>

    <span data-ttu-id="6b1f1-184">Alla prima esecuzione di una pagina che accede al database, Entity Framework esegue tutte le migrazioni `Up` metodi necessari per portare il database aggiornato con il modello di dati corrente.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-184">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span> <span data-ttu-id="6b1f1-185">È ora possibile usare tutte le pagine web che è stato aggiunto dopo l'ultima che è stata distribuita, incluse le pagine di reparto che è stato aggiunto in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-185">You can now use all of the web pages that you added since the last time you deployed, including the Department pages that you added in this tutorial.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="6b1f1-186">Ottenere il codice</span><span class="sxs-lookup"><span data-stu-id="6b1f1-186">Get the code</span></span>

[<span data-ttu-id="6b1f1-187">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="6b1f1-187">Download the Completed Project</span></span>](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a><span data-ttu-id="6b1f1-188">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6b1f1-188">Additional resources</span></span>

<span data-ttu-id="6b1f1-189">Sono disponibili collegamenti ad altre risorse di Entity Framework nel [l'accesso ai dati ASP.NET - risorse consigliate](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="6b1f1-189">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6b1f1-190">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="6b1f1-190">Next steps</span></span>

<span data-ttu-id="6b1f1-191">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="6b1f1-191">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6b1f1-192">Imparato a codice asincrono</span><span class="sxs-lookup"><span data-stu-id="6b1f1-192">Learned about asynchronous code</span></span>
> * <span data-ttu-id="6b1f1-193">Creazione di un controller di reparto</span><span class="sxs-lookup"><span data-stu-id="6b1f1-193">Created a Department controller</span></span>
> * <span data-ttu-id="6b1f1-194">Utilizzare le stored procedure</span><span class="sxs-lookup"><span data-stu-id="6b1f1-194">Used stored procedures</span></span>
> * <span data-ttu-id="6b1f1-195">Distribuito in Azure</span><span class="sxs-lookup"><span data-stu-id="6b1f1-195">Deployed to Azure</span></span>

<span data-ttu-id="6b1f1-196">Passare all'articolo successivo per informazioni su come gestire i conflitti quando più utenti aggiornano la stessa entità contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="6b1f1-196">Advance to the next article to learn how to handle conflicts when multiple users update the same entity at the same time.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="6b1f1-197">Gestione della concorrenza</span><span class="sxs-lookup"><span data-stu-id="6b1f1-197">Handling concurrency</span></span>](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
