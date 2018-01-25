---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'Database di Entity Framework prima con ASP.NET MVC: miglioramento della convalida dei dati | Documenti Microsoft'
author: tfitzmac
description: "Usa lo Scaffolding di ASP.NET MVC ed Entity Framework, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa esercitazione seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 842496c2d3ec56fb81f2409dd7d05d800f83799b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a><span data-ttu-id="38d4e-104">Database di Entity Framework prima con ASP.NET MVC: miglioramento della convalida dei dati</span><span class="sxs-lookup"><span data-stu-id="38d4e-104">EF Database First with ASP.NET MVC: Enhancing Data Validation</span></span>
====================
<span data-ttu-id="38d4e-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="38d4e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="38d4e-106">Usa lo Scaffolding di ASP.NET MVC ed Entity Framework, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente.</span><span class="sxs-lookup"><span data-stu-id="38d4e-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="38d4e-107">Questa serie di esercitazioni viene illustrato come automaticamente generare il codice che consente agli utenti di visualizzare, modificare, creare ed eliminare dati che si trovano in una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="38d4e-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="38d4e-108">Il codice generato corrisponde alle colonne nella tabella di database.</span><span class="sxs-lookup"><span data-stu-id="38d4e-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="38d4e-109">Questa parte della serie è incentrata sull'aggiunta di annotazioni di dati al modello di dati per specificare i requisiti della convalida e formattazione di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="38d4e-109">This part of the series focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="38d4e-110">È stato migliorato in base ai suggerimenti degli utenti nella sezione dei commenti.</span><span class="sxs-lookup"><span data-stu-id="38d4e-110">It was improved based on feedback from users in the comments section.</span></span>


## <a name="add-data-annotations"></a><span data-ttu-id="38d4e-111">Aggiungere le annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="38d4e-111">Add data annotations</span></span>

<span data-ttu-id="38d4e-112">Come osservato nell'argomento precedente, vengono applicate automaticamente alcune regole di convalida dei dati per l'input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="38d4e-112">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="38d4e-113">Ad esempio, è possibile fornire solo un numero per la proprietà livello.</span><span class="sxs-lookup"><span data-stu-id="38d4e-113">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="38d4e-114">Per specificare più regole di convalida di dati, è possibile aggiungere le annotazioni dei dati per la classe di modello.</span><span class="sxs-lookup"><span data-stu-id="38d4e-114">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="38d4e-115">Queste annotazioni vengono applicate in tutta l'applicazione web per la proprietà specificata.</span><span class="sxs-lookup"><span data-stu-id="38d4e-115">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="38d4e-116">È inoltre possibile applicare gli attributi di formattazione che modificano la modalità vengono visualizzate le proprietà. ad esempio, la modifica del valore utilizzato per le etichette di testo.</span><span class="sxs-lookup"><span data-stu-id="38d4e-116">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="38d4e-117">In questa esercitazione si aggiungerà le annotazioni dei dati per limitare la lunghezza dei valori forniti per le proprietà di FirstName, LastName e MiddleName.</span><span class="sxs-lookup"><span data-stu-id="38d4e-117">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="38d4e-118">Nel database, questi valori sono limitati a 50 caratteri. Tuttavia, nell'applicazione web tale limite di caratteri attualmente non viene applicata.</span><span class="sxs-lookup"><span data-stu-id="38d4e-118">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="38d4e-119">Se un utente fornisce più di 50 caratteri per uno di questi valori, la pagina verrà arrestato in modo anomalo durante il tentativo di salvare il valore per il database.</span><span class="sxs-lookup"><span data-stu-id="38d4e-119">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="38d4e-120">Livello si limiterà anche per i valori compresi tra 0 e 4.</span><span class="sxs-lookup"><span data-stu-id="38d4e-120">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="38d4e-121">Aprire il **Student.cs** file nel **modelli** cartella.</span><span class="sxs-lookup"><span data-stu-id="38d4e-121">Open the **Student.cs** file in the **Models** folder.</span></span> <span data-ttu-id="38d4e-122">Aggiungere il codice evidenziato seguente alla classe.</span><span class="sxs-lookup"><span data-stu-id="38d4e-122">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="38d4e-123">In Enrollment.cs, aggiungere il codice evidenziato di seguito.</span><span class="sxs-lookup"><span data-stu-id="38d4e-123">In Enrollment.cs, add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="38d4e-124">Compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="38d4e-124">Build the solution.</span></span>

<span data-ttu-id="38d4e-125">Passare a una pagina per la modifica o la creazione di uno studente.</span><span class="sxs-lookup"><span data-stu-id="38d4e-125">Browse to a page for editing or creating a student.</span></span> <span data-ttu-id="38d4e-126">Se si tenta di immettere più di 50 caratteri, viene visualizzato un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="38d4e-126">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![Mostra messaggio di errore](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="38d4e-128">Passare alla pagina per modificare le registrazioni e tenta di fornire un livello sopra 4.</span><span class="sxs-lookup"><span data-stu-id="38d4e-128">Browse to the page for editing enrollments, and attempt to provide a grade above 4.</span></span>

![Errore di intervallo di livello](enhancing-data-validation/_static/image2.png)

<span data-ttu-id="38d4e-130">Per un elenco completo delle annotazioni di convalida dei dati è possibile applicare alle proprietà e le classi, vedere [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span><span class="sxs-lookup"><span data-stu-id="38d4e-130">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="38d4e-131">Aggiungere le classi di metadati</span><span class="sxs-lookup"><span data-stu-id="38d4e-131">Add metadata classes</span></span>

<span data-ttu-id="38d4e-132">L'aggiunta degli attributi di convalida direttamente alla classe di modello funziona se non si prevede il database da modificare; Tuttavia, se il database viene modificato ed è necessario rigenerare la classe modello, si perderanno tutti gli attributi che sono stati applicati alla classe di modello.</span><span class="sxs-lookup"><span data-stu-id="38d4e-132">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="38d4e-133">Questo approccio può essere molto inefficiente e soggetto a perdere le regole di convalida importanti.</span><span class="sxs-lookup"><span data-stu-id="38d4e-133">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="38d4e-134">Per evitare questo problema, è possibile aggiungere una classe di metadati che contiene gli attributi.</span><span class="sxs-lookup"><span data-stu-id="38d4e-134">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="38d4e-135">Quando si associa la classe di modello per la classe di metadati, tali attributi vengono applicati al modello.</span><span class="sxs-lookup"><span data-stu-id="38d4e-135">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="38d4e-136">In questo caso, la classe modello può essere rigenerata senza perdere tutti gli attributi che sono stati applicati alla classe di metadati.</span><span class="sxs-lookup"><span data-stu-id="38d4e-136">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="38d4e-137">Nel **modelli** cartella, aggiungere una classe denominata **Metadata.cs**.</span><span class="sxs-lookup"><span data-stu-id="38d4e-137">In the **Models** folder, add a class named **Metadata.cs**.</span></span>

![aggiungere una classe di metadati](enhancing-data-validation/_static/image3.png)

<span data-ttu-id="38d4e-139">Sostituire il codice in Metadata.cs con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="38d4e-139">Replace the code in Metadata.cs with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="38d4e-140">Queste classi di metadati contengano tutti gli attributi di convalida che è stato applicato precedentemente a classi del modello.</span><span class="sxs-lookup"><span data-stu-id="38d4e-140">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="38d4e-141">Il **visualizzazione** attributo viene utilizzato per modificare il valore utilizzato per le etichette di testo.</span><span class="sxs-lookup"><span data-stu-id="38d4e-141">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="38d4e-142">A questo punto, è necessario associare le classi di modello con le classi dei metadati.</span><span class="sxs-lookup"><span data-stu-id="38d4e-142">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="38d4e-143">Nel **modelli** cartella, aggiungere una classe denominata **PartialClasses.cs**.</span><span class="sxs-lookup"><span data-stu-id="38d4e-143">In the **Models** folder, add a class named **PartialClasses.cs**.</span></span>

<span data-ttu-id="38d4e-144">Sostituire il contenuto del file con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="38d4e-144">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="38d4e-145">Si noti che ogni classe è contrassegnata come un `partial` classe e ogni lo stesso nome e spazio dei nomi della classe generata automaticamente.</span><span class="sxs-lookup"><span data-stu-id="38d4e-145">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="38d4e-146">Applicando l'attributo dei metadati per la classe parziale, assicurarsi che gli attributi di convalida di dati verranno applicati alla classe generata automaticamente.</span><span class="sxs-lookup"><span data-stu-id="38d4e-146">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="38d4e-147">Questi attributi non andranno persi quando si rigenerano le classi di modello, poiché viene applicato l'attributo di metadati in classi parziali che non vengono rigenerate.</span><span class="sxs-lookup"><span data-stu-id="38d4e-147">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="38d4e-148">Per rigenerare le classi generate automaticamente, aprire il file ContosoModel.edmx.</span><span class="sxs-lookup"><span data-stu-id="38d4e-148">To regenerate the automatically-generated classes, open the ContosoModel.edmx file.</span></span> <span data-ttu-id="38d4e-149">In questo caso, fare clic su area di progettazione e seleziona **il modello di aggiornamento dal Database**.</span><span class="sxs-lookup"><span data-stu-id="38d4e-149">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="38d4e-150">Anche se non è stato modificato il database, questo processo verrà rigenerata le classi.</span><span class="sxs-lookup"><span data-stu-id="38d4e-150">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="38d4e-151">Nel **aggiornamento** , selezionare **tabelle** e **fine**.</span><span class="sxs-lookup"><span data-stu-id="38d4e-151">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

![aggiornare le tabelle](enhancing-data-validation/_static/image4.png)

<span data-ttu-id="38d4e-153">Salvare il file ContosoModel.edmx per applicare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="38d4e-153">Save the ContosoModel.edmx file to apply the changes.</span></span>

<span data-ttu-id="38d4e-154">Aprire il file Student.cs o il file Enrollment.cs e si noti che gli attributi di convalida dei dati applicate in precedenza non sono più nel file.</span><span class="sxs-lookup"><span data-stu-id="38d4e-154">Open the Student.cs file or the Enrollment.cs file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="38d4e-155">Tuttavia, eseguire l'applicazione e si noti che quando si immettono dati ancora vengono applicate le regole di convalida.</span><span class="sxs-lookup"><span data-stu-id="38d4e-155">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="38d4e-156">[Precedente](customizing-a-view.md)
[Successivo](publish-to-azure.md)</span><span class="sxs-lookup"><span data-stu-id="38d4e-156">[Previous](customizing-a-view.md)
[Next](publish-to-azure.md)</span></span>
