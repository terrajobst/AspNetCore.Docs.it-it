---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'Esercitazione: Migliorare la convalida dei dati per Entity Framework Database First con app ASP.NET MVC'
description: Questa esercitazione è incentrata sull'aggiunta di annotazioni dei dati al modello di dati per specificare i requisiti della convalida e formattazione di visualizzazione.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 85299d70c6cba52c1d40a42edfd429c96318134a
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236484"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="b689f-103">Esercitazione: Migliorare la convalida dei dati per Entity Framework Database First con app ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b689f-103">Tutorial: Enhance data validation for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="b689f-104">Usa MVC, Entity Framework e lo Scaffolding di ASP.NET, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente.</span><span class="sxs-lookup"><span data-stu-id="b689f-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="b689f-105">Questa serie di esercitazioni illustra come generare il codice che consente agli utenti di visualizzare, modificare, creare automaticamente ed eliminare dati che si trovano in una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="b689f-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="b689f-106">Il codice generato corrispondente alle colonne nella tabella di database.</span><span class="sxs-lookup"><span data-stu-id="b689f-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="b689f-107">Questa esercitazione è incentrata sull'aggiunta di annotazioni dei dati al modello di dati per specificare i requisiti della convalida e formattazione di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="b689f-107">This tutorial focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="b689f-108">È stato migliorato in base al feedback degli utenti nella sezione dei commenti.</span><span class="sxs-lookup"><span data-stu-id="b689f-108">It was improved based on feedback from users in the comments section.</span></span>

<span data-ttu-id="b689f-109">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="b689f-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b689f-110">Aggiungere annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="b689f-110">Add data annotations</span></span>
> * <span data-ttu-id="b689f-111">Aggiungere classi di metadati</span><span class="sxs-lookup"><span data-stu-id="b689f-111">Add metadata classes</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b689f-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="b689f-112">Prerequisites</span></span>

* [<span data-ttu-id="b689f-113">Personalizzare una vista</span><span class="sxs-lookup"><span data-stu-id="b689f-113">Customize a view</span></span>](customizing-a-view.md)

## <a name="add-data-annotations"></a><span data-ttu-id="b689f-114">Aggiungere annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="b689f-114">Add data annotations</span></span>

<span data-ttu-id="b689f-115">Come illustrato in un argomento precedente, alcune regole di convalida dei dati vengono automaticamente applicate agli input dell'utente.</span><span class="sxs-lookup"><span data-stu-id="b689f-115">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="b689f-116">Ad esempio, è possibile fornire solo un numero per la proprietà di livello.</span><span class="sxs-lookup"><span data-stu-id="b689f-116">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="b689f-117">Per specificare più regole di convalida dei dati, è possibile aggiungere annotazioni dei dati per la classe di modello.</span><span class="sxs-lookup"><span data-stu-id="b689f-117">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="b689f-118">Queste annotazioni vengono applicate in tutta l'applicazione web per la proprietà specificata.</span><span class="sxs-lookup"><span data-stu-id="b689f-118">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="b689f-119">È inoltre possibile applicare gli attributi di formattazione che modificano la modalità di visualizzazione di proprietà; ad esempio, la modifica del valore usato per le etichette di testo.</span><span class="sxs-lookup"><span data-stu-id="b689f-119">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="b689f-120">In questa esercitazione si aggiungerà le annotazioni dei dati per limitare la lunghezza dei valori forniti per le proprietà FirstName, LastName e MiddleName.</span><span class="sxs-lookup"><span data-stu-id="b689f-120">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="b689f-121">Nel database, questi valori sono limitati a 50 caratteri. Tuttavia, nell'applicazione web tale limite di caratteri attualmente non viene applicata.</span><span class="sxs-lookup"><span data-stu-id="b689f-121">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="b689f-122">Se si specificano più di 50 caratteri per uno di questi valori, la pagina viene interrotta quando prova a salvare il valore per il database.</span><span class="sxs-lookup"><span data-stu-id="b689f-122">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="b689f-123">Livello si limiterà anche in valori compresi tra 0 e 4.</span><span class="sxs-lookup"><span data-stu-id="b689f-123">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="b689f-124">Selezionare **modelli** > **ContosoModel.edmx** > **ContosoModel.tt** e aprire il *Student.cs* file.</span><span class="sxs-lookup"><span data-stu-id="b689f-124">Select **Models** > **ContosoModel.edmx** > **ContosoModel.tt** and open the *Student.cs* file.</span></span> <span data-ttu-id="b689f-125">Aggiungere il codice evidenziato seguente alla classe.</span><span class="sxs-lookup"><span data-stu-id="b689f-125">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="b689f-126">Aprire *Enrollment.cs* e aggiungere il codice evidenziato seguente.</span><span class="sxs-lookup"><span data-stu-id="b689f-126">Open *Enrollment.cs* and add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="b689f-127">Compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="b689f-127">Build the solution.</span></span>

<span data-ttu-id="b689f-128">Fare clic su **elenco degli studenti** e selezionare **modificare**.</span><span class="sxs-lookup"><span data-stu-id="b689f-128">Click **List of students** and select **Edit**.</span></span> <span data-ttu-id="b689f-129">Se si tenta di immettere più di 50 caratteri, viene visualizzato un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="b689f-129">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![Mostra messaggio di errore](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="b689f-131">Tornare alla home page.</span><span class="sxs-lookup"><span data-stu-id="b689f-131">Go back to the home page.</span></span> <span data-ttu-id="b689f-132">Fare clic su **elenco di registrazioni** e selezionare **modificare**.</span><span class="sxs-lookup"><span data-stu-id="b689f-132">Click **List of enrollments** and select **Edit**.</span></span> <span data-ttu-id="b689f-133">Tentativo di fornire un livello superiori a 4.</span><span class="sxs-lookup"><span data-stu-id="b689f-133">Attempt to provide a grade above 4.</span></span> <span data-ttu-id="b689f-134">Si riceverà questo errore: *Il campo livello deve essere compreso tra 0 e 4.*</span><span class="sxs-lookup"><span data-stu-id="b689f-134">You will receive this error: *The field Grade must be between 0 and 4.*</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="b689f-135">Aggiungere classi di metadati</span><span class="sxs-lookup"><span data-stu-id="b689f-135">Add metadata classes</span></span>

<span data-ttu-id="b689f-136">L'aggiunta di attributi di convalida direttamente alla classe di modello funziona quando non si prevede il database da modificare; Tuttavia, se il database viene modificato ed è necessario rigenerare la classe del modello, si perderanno tutti gli attributi applicati alla classe di modello.</span><span class="sxs-lookup"><span data-stu-id="b689f-136">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="b689f-137">Questo approccio può essere molto inefficiente e soggetto a perdere le regole di convalida importanti.</span><span class="sxs-lookup"><span data-stu-id="b689f-137">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="b689f-138">Per evitare questo problema, è possibile aggiungere una classe di metadati che contiene gli attributi.</span><span class="sxs-lookup"><span data-stu-id="b689f-138">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="b689f-139">Quando si associa la classe del modello alla classe di metadati, tali attributi vengono applicati al modello.</span><span class="sxs-lookup"><span data-stu-id="b689f-139">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="b689f-140">Questo approccio, la classe modello può essere rigenerata senza perdita di tutti gli attributi che sono stati applicati alla classe di metadati.</span><span class="sxs-lookup"><span data-stu-id="b689f-140">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="b689f-141">Nel **modelli** cartella, aggiungere una classe denominata *Metadata.cs*.</span><span class="sxs-lookup"><span data-stu-id="b689f-141">In the **Models** folder, add a class named *Metadata.cs*.</span></span>

<span data-ttu-id="b689f-142">Sostituire il codice nel *Metadata.cs* con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="b689f-142">Replace the code in *Metadata.cs* with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="b689f-143">Queste classi di metadati contengono tutti gli attributi di convalida applicati in precedenza per le classi del modello.</span><span class="sxs-lookup"><span data-stu-id="b689f-143">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="b689f-144">Il **Display** attributo viene usato per modificare il valore usato per le etichette di testo.</span><span class="sxs-lookup"><span data-stu-id="b689f-144">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="b689f-145">A questo punto, è necessario associare le classi del modello con le classi di metadati.</span><span class="sxs-lookup"><span data-stu-id="b689f-145">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="b689f-146">Nel **modelli** cartella, aggiungere una classe denominata *PartialClasses.cs*.</span><span class="sxs-lookup"><span data-stu-id="b689f-146">In the **Models** folder, add a class named *PartialClasses.cs*.</span></span>

<span data-ttu-id="b689f-147">Sostituire il contenuto del file con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="b689f-147">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="b689f-148">Si noti che ogni classe è contrassegnata come un `partial` classe e ogni corrisponde al nome e lo spazio dei nomi della classe generata automaticamente.</span><span class="sxs-lookup"><span data-stu-id="b689f-148">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="b689f-149">Applicando l'attributo di metadati alla classe parziale, assicurarsi che gli attributi di convalida dei dati saranno essere applicati alla classe generata automaticamente.</span><span class="sxs-lookup"><span data-stu-id="b689f-149">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="b689f-150">Questi attributi non andranno persi quando si rigenerano le classi del modello perché viene applicato l'attributo di metadati nelle classi parziali che non vengono rigenerate.</span><span class="sxs-lookup"><span data-stu-id="b689f-150">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="b689f-151">Per rigenerare le classi generate automaticamente, aprire il *ContosoModel.edmx* file.</span><span class="sxs-lookup"><span data-stu-id="b689f-151">To regenerate the automatically-generated classes, open the *ContosoModel.edmx* file.</span></span> <span data-ttu-id="b689f-152">Ancora una volta, fare clic su area di progettazione e seleziona **Aggiorna modello da Database**.</span><span class="sxs-lookup"><span data-stu-id="b689f-152">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="b689f-153">Anche se non sono stati modificati del database, questo processo verrà rigenerare le classi.</span><span class="sxs-lookup"><span data-stu-id="b689f-153">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="b689f-154">Nel **Refresh** scheda, seleziona **tabelle** e **fine**.</span><span class="sxs-lookup"><span data-stu-id="b689f-154">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

<span data-ttu-id="b689f-155">Salvare il *ContosoModel.edmx* file per applicare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="b689f-155">Save the *ContosoModel.edmx* file to apply the changes.</span></span>

<span data-ttu-id="b689f-156">Aprire il *Student.cs* file o la *Enrollment.cs* file e notare che gli attributi di convalida dei dati è stato applicato in precedenza non sono più nel file.</span><span class="sxs-lookup"><span data-stu-id="b689f-156">Open the *Student.cs* file or the *Enrollment.cs* file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="b689f-157">Tuttavia, eseguire l'applicazione e si noti che le regole di convalida vengono comunque applicate quando si immettono dati.</span><span class="sxs-lookup"><span data-stu-id="b689f-157">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b689f-158">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b689f-158">Additional resources</span></span>

<span data-ttu-id="b689f-159">Per un elenco completo delle annotazioni di convalida dei dati è possibile applicare alle proprietà e classi, vedere [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span><span class="sxs-lookup"><span data-stu-id="b689f-159">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b689f-160">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="b689f-160">Next steps</span></span>

<span data-ttu-id="b689f-161">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="b689f-161">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b689f-162">Annotazioni di dati aggiunti</span><span class="sxs-lookup"><span data-stu-id="b689f-162">Added data annotations</span></span>
> * <span data-ttu-id="b689f-163">Classi di metadati aggiunti</span><span class="sxs-lookup"><span data-stu-id="b689f-163">Added metadata classes</span></span>

<span data-ttu-id="b689f-164">Passare all'esercitazione successiva per informazioni su come pubblicare l'app web e database in Azure.</span><span class="sxs-lookup"><span data-stu-id="b689f-164">Advance to the next tutorial to learn how to publish the web app and database to Azure.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="b689f-165">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="b689f-165">Publish to Azure</span></span>](publish-to-azure.md)