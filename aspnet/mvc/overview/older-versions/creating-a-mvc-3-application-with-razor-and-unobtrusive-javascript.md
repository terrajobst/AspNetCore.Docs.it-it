---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Creazione di un MVC 3 Application with Razor and JavaScript non intrusivo | Documenti Microsoft
author: microsoft
description: L'applicazione web di esempio elenco utenti viene illustrato quanto sia semplice per creare applicazioni ASP.NET MVC 3 con il motore di visualizzazione Razor. S l'applicazione di esempio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 9b273f6827cad2078b581d6da7b127198dfddaa5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2018
ms.locfileid: "30874694"
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a><span data-ttu-id="61c2e-104">Creazione di un MVC 3 Application with Razor and JavaScript non intrusivo</span><span class="sxs-lookup"><span data-stu-id="61c2e-104">Creating a MVC 3 Application with Razor and Unobtrusive JavaScript</span></span>
====================
<span data-ttu-id="61c2e-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="61c2e-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="61c2e-106">L'applicazione web di esempio elenco utenti viene illustrato quanto sia semplice per creare applicazioni ASP.NET MVC 3 con il motore di visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="61c2e-106">The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine.</span></span> <span data-ttu-id="61c2e-107">L'applicazione di esempio viene illustrato come utilizzare il nuovo motore di visualizzazione Razor con ASP.NET MVC versione 3 e Visual Studio 2010 per creare un sito Web di elenco degli utenti fittizi che include funzionalità, quali la creazione, visualizzazione, modifica ed eliminazione degli utenti.</span><span class="sxs-lookup"><span data-stu-id="61c2e-107">The sample application shows how to use the new Razor view engine with ASP.NET MVC version 3 and Visual Studio 2010 to create a fictional User List website that includes functionality such as creating, displaying, editing, and deleting users.</span></span>
> 
> <span data-ttu-id="61c2e-108">In questa esercitazione vengono descritti i passaggi che sono stati eseguiti per compilare l'applicazione ASP.NET MVC 3 di esempio elenco utenti.</span><span class="sxs-lookup"><span data-stu-id="61c2e-108">This tutorial describes the steps that were taken in order to build the User List sample ASP.NET MVC 3 application.</span></span> <span data-ttu-id="61c2e-109">Un progetto di Visual Studio con c# e VB codice sorgente è disponibile a complemento di questo argomento: [scaricare](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span><span class="sxs-lookup"><span data-stu-id="61c2e-109">A Visual Studio project with C# and VB source code is available to accompany this topic: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span></span> <span data-ttu-id="61c2e-110">Nel caso di domande su questa esercitazione, post per il [forum MVC](https://forums.asp.net/1146.aspx).</span><span class="sxs-lookup"><span data-stu-id="61c2e-110">If you have questions about this tutorial, please post them to the [MVC forum](https://forums.asp.net/1146.aspx).</span></span>


## <a name="overview"></a><span data-ttu-id="61c2e-111">Panoramica</span><span class="sxs-lookup"><span data-stu-id="61c2e-111">Overview</span></span>

<span data-ttu-id="61c2e-112">L'applicazione da generare è un semplice utente elenco sito Web.</span><span class="sxs-lookup"><span data-stu-id="61c2e-112">The application you'll be building is a simple user list website.</span></span> <span data-ttu-id="61c2e-113">Gli utenti possono immettere, visualizzare e aggiornare le informazioni utente.</span><span class="sxs-lookup"><span data-stu-id="61c2e-113">Users can enter, view, and update user information.</span></span>

![Sito di esempio](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

<span data-ttu-id="61c2e-115">È possibile scaricare il progetto completato VB e c# [qui](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span><span class="sxs-lookup"><span data-stu-id="61c2e-115">You can download the VB and C# completed project [here](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span></span>

## <a name="creating-the-web-application"></a><span data-ttu-id="61c2e-116">Creazione dell'applicazione Web</span><span class="sxs-lookup"><span data-stu-id="61c2e-116">Creating the Web Application</span></span>

<span data-ttu-id="61c2e-117">Per avviare l'esercitazione, aprire Visual Studio 2010 e creare un nuovo progetto utilizzando il *applicazione Web di ASP.NET MVC 3* modello.</span><span class="sxs-lookup"><span data-stu-id="61c2e-117">To start the tutorial, open Visual Studio 2010 and create a new project using the *ASP.NET MVC 3 Web Application* template.</span></span> <span data-ttu-id="61c2e-118">Nome dell'applicazione &quot;Mvc3Razor&quot;.</span><span class="sxs-lookup"><span data-stu-id="61c2e-118">Name the application &quot;Mvc3Razor&quot;.</span></span>

<span data-ttu-id="61c2e-119">[![Nuovo progetto MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="61c2e-119">[![New MVC 3 project](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span></span>

<span data-ttu-id="61c2e-120">Nel **nuovo progetto ASP.NET MVC 3** finestra di dialogo Seleziona **applicazione Internet**, selezionare il motore di visualizzazione Razor e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="61c2e-120">In the **New ASP.NET MVC 3 Project** dialog, select **Internet Application**, select the Razor view engine, and then click **OK**.</span></span>

![Finestra di dialogo Nuovo progetto ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

<span data-ttu-id="61c2e-122">In questa esercitazione non si utilizzerà il provider di appartenenze ASP.NET, pertanto è possibile eliminare tutti i file associati all'accesso e l'appartenenza.</span><span class="sxs-lookup"><span data-stu-id="61c2e-122">In this tutorial you will not be using the ASP.NET membership provider, so you can delete all the files associated with logon and membership.</span></span> <span data-ttu-id="61c2e-123">In **Esplora**, rimuovere i file e directory seguenti:</span><span class="sxs-lookup"><span data-stu-id="61c2e-123">In **Solution Explorer**, remove the following files and directories:</span></span>

- <span data-ttu-id="61c2e-124">*Controllers\AccountController*</span><span class="sxs-lookup"><span data-stu-id="61c2e-124">*Controllers\AccountController*</span></span>
- <span data-ttu-id="61c2e-125">*Models\AccountModels*</span><span class="sxs-lookup"><span data-stu-id="61c2e-125">*Models\AccountModels*</span></span>
- <span data-ttu-id="61c2e-126">*Views\Shared\\_LogOnPartial*</span><span class="sxs-lookup"><span data-stu-id="61c2e-126">*Views\Shared\\_LogOnPartial*</span></span>
- <span data-ttu-id="61c2e-127">*Views\Account* (e tutti i file in questa directory)</span><span class="sxs-lookup"><span data-stu-id="61c2e-127">*Views\Account* (and all the files in this directory)</span></span>

![Exp soln](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

<span data-ttu-id="61c2e-129">Modificare il  <em>\_cshtml</em> file e sostituire il markup all'interno di `<div>` elemento denominato `logindisplay` con il messaggio <em>&quot;</em>account di accesso disabilitato&quot;.</span><span class="sxs-lookup"><span data-stu-id="61c2e-129">Edit the <em>\_Layout.cshtml</em> file and replace the markup inside the `<div>` element named `logindisplay` with the message <em>&quot;</em>Login Disabled&quot;.</span></span> <span data-ttu-id="61c2e-130">Nell'esempio seguente viene illustrato il markup di nuovo:</span><span class="sxs-lookup"><span data-stu-id="61c2e-130">The following example shows the new markup:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a><span data-ttu-id="61c2e-131">Aggiunta del modello</span><span class="sxs-lookup"><span data-stu-id="61c2e-131">Adding the Model</span></span>

<span data-ttu-id="61c2e-132">In **Esplora**, fare doppio clic su di *modelli* cartella, selezionare **Aggiungi**e quindi fare clic su **classe**.</span><span class="sxs-lookup"><span data-stu-id="61c2e-132">In **Solution Explorer**, right-click the *Models* folder, select **Add**, and then click **Class**.</span></span>

![Nuova classe Mdl utente](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

<span data-ttu-id="61c2e-134">Assegnare alla classe il nome `UserModel`.</span><span class="sxs-lookup"><span data-stu-id="61c2e-134">Name the class `UserModel`.</span></span> <span data-ttu-id="61c2e-135">Sostituire il contenuto del *UserModel* file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="61c2e-135">Replace the contents of the *UserModel* file with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

<span data-ttu-id="61c2e-136">La `UserModel` classe rappresenta gli utenti.</span><span class="sxs-lookup"><span data-stu-id="61c2e-136">The `UserModel` class represents users.</span></span> <span data-ttu-id="61c2e-137">Ogni membro della classe è annotato con il [necessari](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) dall'attributo di [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="61c2e-137">Each member of the class is annotated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute from the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace.</span></span> <span data-ttu-id="61c2e-138">Gli attributi di [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) dello spazio dei nomi forniscono la convalida lato client e server automatica per le applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="61c2e-138">The attributes in the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provide automatic client- and server-side validation for web applications.</span></span>

<span data-ttu-id="61c2e-139">Aprire il `HomeController` classe e aggiungere un `using` direttiva in modo da poter accedere il `UserModel` e `Users` classi:</span><span class="sxs-lookup"><span data-stu-id="61c2e-139">Open the `HomeController` class and add a `using` directive so that you can access the `UserModel` and `Users` classes:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

<span data-ttu-id="61c2e-140">Subito dopo il `HomeController` dichiarazione, aggiungere il seguente commento e il riferimento a un `Users` classe:</span><span class="sxs-lookup"><span data-stu-id="61c2e-140">Just after the `HomeController` declaration, add the following comment and the reference to a `Users` class:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

<span data-ttu-id="61c2e-141">La `Users` classe è un archivio di dati semplificato e in memoria che verrà utilizzato in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="61c2e-141">The `Users` class is a simplified, in-memory data store that you'll use in this tutorial.</span></span> <span data-ttu-id="61c2e-142">In un'applicazione reale si utilizzerebbe un database per archiviare informazioni utente.</span><span class="sxs-lookup"><span data-stu-id="61c2e-142">In a real application you would use a database to store user information.</span></span> <span data-ttu-id="61c2e-143">Le prime righe del `HomeController` file presenti nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="61c2e-143">The first few lines of the `HomeController` file are shown in the following example:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

<span data-ttu-id="61c2e-144">Compilare l'applicazione in modo che il modello di utente sarà disponibile per il procedura guidata lo scaffolding nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="61c2e-144">Build the application so that the user model will be available to the scaffolding wizard in the next step.</span></span>

## <a name="creating-the-default-view"></a><span data-ttu-id="61c2e-145">Creazione della visualizzazione predefinita</span><span class="sxs-lookup"><span data-stu-id="61c2e-145">Creating the Default View</span></span>

<span data-ttu-id="61c2e-146">Il passaggio successivo consiste nell'aggiungere un metodo di azione e la visualizzazione per visualizzare gli utenti.</span><span class="sxs-lookup"><span data-stu-id="61c2e-146">The next step is to add an action method and view to display the users.</span></span>

<span data-ttu-id="61c2e-147">Eliminare quello esistente *Views\Home\Index* file.</span><span class="sxs-lookup"><span data-stu-id="61c2e-147">Delete the existing *Views\Home\Index* file.</span></span> <span data-ttu-id="61c2e-148">Verrà creato un nuovo *indice* file per visualizzare gli utenti.</span><span class="sxs-lookup"><span data-stu-id="61c2e-148">You will create a new *Index* file to display the users.</span></span>

<span data-ttu-id="61c2e-149">Nel `HomeController` classe, sostituire il contenuto del `Index` (metodo) con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="61c2e-149">In the `HomeController` class, replace the contents of the `Index` method with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

<span data-ttu-id="61c2e-150">Fare doppio clic all'interno di `Index` (metodo) e quindi fare clic su **Aggiungi visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="61c2e-150">Right-click inside the `Index` method and then click **Add View**.</span></span>

![Aggiungi visualizzazione](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

<span data-ttu-id="61c2e-152">Selezionare il **creare una visualizzazione fortemente tipizzata** opzione.</span><span class="sxs-lookup"><span data-stu-id="61c2e-152">Select the **Create a strongly-typed view** option.</span></span> <span data-ttu-id="61c2e-153">Per **visualizzare dati classe**selezionare **Mvc3Razor.Models.UserModel**.</span><span class="sxs-lookup"><span data-stu-id="61c2e-153">For **View data class**, select **Mvc3Razor.Models.UserModel**.</span></span> <span data-ttu-id="61c2e-154">(Se non viene visualizzato **Mvc3Razor.Models.UserModel** nel **visualizzare dati classe** casella, è necessario compilare il progetto.) Assicurarsi che il motore di visualizzazione è impostato su **Razor**.</span><span class="sxs-lookup"><span data-stu-id="61c2e-154">(If you don't see **Mvc3Razor.Models.UserModel** in the **View data class** box, you need to build the project.) Make sure that the view engine is set to **Razor**.</span></span> <span data-ttu-id="61c2e-155">Impostare **visualizzare il contenuto** a **elenco** e quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="61c2e-155">Set **View content** to **List** and then click **Add**.</span></span>

![Aggiungere una visualizzazione dell'indice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

<span data-ttu-id="61c2e-157">La nuova vista scaffolds automaticamente i dati utente che viene passati per il `Index` visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="61c2e-157">The new view automatically scaffolds the user data that's passed to the `Index` view.</span></span> <span data-ttu-id="61c2e-158">Esaminare appena generato *Views\Home\Index* file.</span><span class="sxs-lookup"><span data-stu-id="61c2e-158">Examine the newly generated *Views\Home\Index* file.</span></span> <span data-ttu-id="61c2e-159">Il **Crea nuovo**, **modifica**, **dettagli**, e **eliminare** collegamenti non funzionano, ma il resto della pagina è funzionale.</span><span class="sxs-lookup"><span data-stu-id="61c2e-159">The **Create New**, **Edit**, **Details**, and **Delete** links don't work, but the rest of the page is functional.</span></span> <span data-ttu-id="61c2e-160">Eseguire la pagina.</span><span class="sxs-lookup"><span data-stu-id="61c2e-160">Run the page.</span></span> <span data-ttu-id="61c2e-161">Verrà visualizzato un elenco di utenti.</span><span class="sxs-lookup"><span data-stu-id="61c2e-161">You see a list of users.</span></span>

![Pagina di indice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

<span data-ttu-id="61c2e-163">Aprire il *cshtml* file e sostituire il `ActionLink` markup per **modifica**, **dettagli**, e **eliminare** con il codice seguente :</span><span class="sxs-lookup"><span data-stu-id="61c2e-163">Open the *Index.cshtml* file and replace the `ActionLink` markup for **Edit**, **Details**, and **Delete** with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

<span data-ttu-id="61c2e-164">Il nome utente è utilizzato come l'ID per trovare il record selezionato nel **modifica**, **dettagli**, e **eliminare** collegamenti.</span><span class="sxs-lookup"><span data-stu-id="61c2e-164">The user name is used as the ID to find the selected record in the **Edit**, **Details**, and **Delete** links.</span></span>

## <a name="creating-the-details-view"></a><span data-ttu-id="61c2e-165">Creazione della visualizzazione dettagli</span><span class="sxs-lookup"><span data-stu-id="61c2e-165">Creating the Details View</span></span>

<span data-ttu-id="61c2e-166">Il passaggio successivo consiste nell'aggiungere un `Details` metodo di azione e visualizzazione per visualizzare i dettagli dell'utente.</span><span class="sxs-lookup"><span data-stu-id="61c2e-166">The next step is to add a `Details` action method and view in order to display user details.</span></span>

![Dettagli](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

<span data-ttu-id="61c2e-168">Aggiungere il seguente `Details` metodo al controller home:</span><span class="sxs-lookup"><span data-stu-id="61c2e-168">Add the following `Details` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

<span data-ttu-id="61c2e-169">Fare doppio clic all'interno di `Details` (metodo) e quindi selezionare <strong>Aggiungi visualizzazione</strong>.</span><span class="sxs-lookup"><span data-stu-id="61c2e-169">Right-click inside the `Details` method and then select <strong>Add View</strong>.</span></span> <span data-ttu-id="61c2e-170">Verificare che il <strong>visualizzare dati classe</strong> finestra contiene <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span><span class="sxs-lookup"><span data-stu-id="61c2e-170">Verify that the <strong>View data class</strong> box contains <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span></span> <span data-ttu-id="61c2e-171">Impostare <strong>visualizzare il contenuto</strong> a <strong>dettagli</strong> e quindi fare clic su <strong>Aggiungi</strong>.</span><span class="sxs-lookup"><span data-stu-id="61c2e-171">Set <strong>View content</strong> to <strong>Details</strong> and then click <strong>Add</strong>.</span></span>

![Aggiungere una visualizzazione dettagli](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

<span data-ttu-id="61c2e-173">Eseguire l'applicazione e selezionare un collegamento di dettagli.</span><span class="sxs-lookup"><span data-stu-id="61c2e-173">Run the application and select a details link.</span></span> <span data-ttu-id="61c2e-174">Lo scaffolding automatico Mostra ogni proprietà nel modello.</span><span class="sxs-lookup"><span data-stu-id="61c2e-174">The automatic scaffolding shows each property in the model.</span></span>

![Dettagli](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a><span data-ttu-id="61c2e-176">Creazione della visualizzazione di modifica</span><span class="sxs-lookup"><span data-stu-id="61c2e-176">Creating the Edit View</span></span>

<span data-ttu-id="61c2e-177">Aggiungere il seguente `Edit` metodo al controller home.</span><span class="sxs-lookup"><span data-stu-id="61c2e-177">Add the following `Edit` method to the home controller.</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

<span data-ttu-id="61c2e-178">Aggiungere una visualizzazione, come illustrato nei passaggi precedenti, ma impostare **visualizzare il contenuto** a **modifica**.</span><span class="sxs-lookup"><span data-stu-id="61c2e-178">Add a view as in the previous steps, but set **View content** to **Edit**.</span></span>

![Aggiungere una visualizzazione di modifica](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

<span data-ttu-id="61c2e-180">Eseguire l'applicazione e modificare il nome e cognome di uno degli utenti.</span><span class="sxs-lookup"><span data-stu-id="61c2e-180">Run the application and edit the first and last name of one of the users.</span></span> <span data-ttu-id="61c2e-181">In caso di violazione qualsiasi `DataAnnotation` vincoli applicati per la `UserModel` (classe), quando si invia il form, si noterà gli errori di convalida generati dal codice server.</span><span class="sxs-lookup"><span data-stu-id="61c2e-181">If you violate any `DataAnnotation` constraints that have been applied to the `UserModel` class, when you submit the form, you will see validation errors that are produced by server code.</span></span> <span data-ttu-id="61c2e-182">Ad esempio, se si modifica il nome &quot;Ann&quot; a &quot;A&quot;, quando si invia il form, nel form viene visualizzato l'errore seguente:</span><span class="sxs-lookup"><span data-stu-id="61c2e-182">For example, if you change the first name &quot;Ann&quot; to &quot;A&quot;, when you submit the form, the following error is displayed on the form:</span></span>

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

<span data-ttu-id="61c2e-183">In questa esercitazione, si sta considerando il nome utente come chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="61c2e-183">In this tutorial, you're treating the user name as the primary key.</span></span> <span data-ttu-id="61c2e-184">Pertanto, la proprietà del nome utente non può essere modificata.</span><span class="sxs-lookup"><span data-stu-id="61c2e-184">Therefore, the user name property cannot be changed.</span></span> <span data-ttu-id="61c2e-185">Nel *Edit.cshtml* file, subito dopo il `Html.BeginForm` istruzione, impostare il nome utente da un campo nascosto.</span><span class="sxs-lookup"><span data-stu-id="61c2e-185">In the *Edit.cshtml* file, just after the `Html.BeginForm` statement, set the user name to be a hidden field.</span></span> <span data-ttu-id="61c2e-186">In questo modo la proprietà deve essere passato nel modello.</span><span class="sxs-lookup"><span data-stu-id="61c2e-186">This causes the property to be passed in the model.</span></span> <span data-ttu-id="61c2e-187">Frammento di codice seguente viene illustrata la posizione del `Hidden` istruzione:</span><span class="sxs-lookup"><span data-stu-id="61c2e-187">The following code fragment shows the placement of the `Hidden` statement:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

<span data-ttu-id="61c2e-188">Sostituire il `TextBoxFor` e `ValidationMessageFor` markup per il nome utente con un `DisplayFor` chiamare.</span><span class="sxs-lookup"><span data-stu-id="61c2e-188">Replace the `TextBoxFor` and `ValidationMessageFor` markup for the user name with a `DisplayFor` call.</span></span> <span data-ttu-id="61c2e-189">Il `DisplayFor` metodo consente di visualizzare la proprietà come un elemento di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="61c2e-189">The `DisplayFor` method displays the property as a read-only element.</span></span> <span data-ttu-id="61c2e-190">Nell'esempio seguente viene illustrato il markup completato.</span><span class="sxs-lookup"><span data-stu-id="61c2e-190">The following example shows the completed markup.</span></span> <span data-ttu-id="61c2e-191">Originale `TextBoxFor` e `ValidationMessageFor` chiamate sono impostate come commento con i caratteri di commento begin ed end commento Razor (`@* *@`)</span><span class="sxs-lookup"><span data-stu-id="61c2e-191">The original `TextBoxFor` and `ValidationMessageFor` calls are commented out with the Razor begin-comment and end-comment characters (`@* *@`)</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a><span data-ttu-id="61c2e-192">Abilitando la convalida lato Client</span><span class="sxs-lookup"><span data-stu-id="61c2e-192">Enabling Client-Side Validation</span></span>

<span data-ttu-id="61c2e-193">Per abilitare la convalida lato client in ASP.NET MVC 3, è necessario impostare due flag ed è necessario includere tre file JavaScript.</span><span class="sxs-lookup"><span data-stu-id="61c2e-193">To enable client-side validation in ASP.NET MVC 3, you must set two flags and you must include three JavaScript files.</span></span>

<span data-ttu-id="61c2e-194">Aprire l'applicazione *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="61c2e-194">Open the application's *Web.config* file.</span></span> <span data-ttu-id="61c2e-195">Verificare `that ClientValidationEnabled` e `UnobtrusiveJavaScriptEnabled` sono impostate su true nelle impostazioni dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="61c2e-195">Verify `that ClientValidationEnabled` and `UnobtrusiveJavaScriptEnabled` are set to true in the application settings.</span></span> <span data-ttu-id="61c2e-196">Nel frammento seguente dalla radice *Web. config* file Mostra le impostazioni corrette:</span><span class="sxs-lookup"><span data-stu-id="61c2e-196">The following fragment from the root *Web.config* file shows the correct settings:</span></span>

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

<span data-ttu-id="61c2e-197">Impostazione `UnobtrusiveJavaScriptEnabled` su true, Abilita non intrusivo Ajax e la convalida del client non intrusiva.</span><span class="sxs-lookup"><span data-stu-id="61c2e-197">Setting `UnobtrusiveJavaScriptEnabled` to true enables unobtrusive Ajax and unobtrusive client validation.</span></span> <span data-ttu-id="61c2e-198">Quando si utilizza la convalida non intrusiva, le regole di convalida vengono convertite in attributi HTML5.</span><span class="sxs-lookup"><span data-stu-id="61c2e-198">When you use unobtrusive validation, the validation rules are turned into HTML5 attributes.</span></span> <span data-ttu-id="61c2e-199">I nomi degli attributi HTML5 può contenere solo lettere minuscole, numeri e trattini.</span><span class="sxs-lookup"><span data-stu-id="61c2e-199">HTML5 attribute names can consist of only lowercase letters, numbers, and dashes.</span></span>

<span data-ttu-id="61c2e-200">Impostazione `ClientValidationEnabled` su true consente la convalida lato client.</span><span class="sxs-lookup"><span data-stu-id="61c2e-200">Setting `ClientValidationEnabled` to true enables client-side validation.</span></span> <span data-ttu-id="61c2e-201">Tramite l'impostazione di queste chiavi nell'applicazione *Web. config* file, si abilita la convalida del client e JavaScript non intrusivo per l'intera applicazione.</span><span class="sxs-lookup"><span data-stu-id="61c2e-201">By setting these keys in the application *Web.config* file, you enable client validation and unobtrusive JavaScript for the entire application.</span></span> <span data-ttu-id="61c2e-202">È inoltre possibile abilitare o disabilitare queste impostazioni nelle singole viste o nei metodi controller utilizzando il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="61c2e-202">You can also enable or disable these settings in individual views or in controller methods using the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

<span data-ttu-id="61c2e-203">È inoltre necessario includere diversi file JavaScript in una visualizzazione sottoposta a rendering.</span><span class="sxs-lookup"><span data-stu-id="61c2e-203">You also need to include several JavaScript files in the rendered view.</span></span> <span data-ttu-id="61c2e-204">È un modo semplice per includere il codice JavaScript in tutte le visualizzazioni per aggiungerli al *Views\Shared\\layout. cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="61c2e-204">An easy way to include the JavaScript in all views is to add them to the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="61c2e-205">Sostituire il `<head>` elemento il  *\_cshtml* file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="61c2e-205">Replace the `<head>` element of the *\_Layout.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

<span data-ttu-id="61c2e-206">I primi due script jQuery sono ospitati da di Microsoft Ajax rete CDN (Content Delivery).</span><span class="sxs-lookup"><span data-stu-id="61c2e-206">The first two jQuery scripts are hosted by the Microsoft Ajax Content Delivery Network (CDN).</span></span> <span data-ttu-id="61c2e-207">È possibile sfruttare la rete CDN di Microsoft Ajax, è possibile migliorare notevolmente il primo numero di passaggi delle prestazioni delle applicazioni.</span><span class="sxs-lookup"><span data-stu-id="61c2e-207">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the first-hit performance of your applications.</span></span>

<span data-ttu-id="61c2e-208">Eseguire l'applicazione e fare clic su un collegamento di modifica.</span><span class="sxs-lookup"><span data-stu-id="61c2e-208">Run the application and click an edit link.</span></span> <span data-ttu-id="61c2e-209">Consente di visualizzare l'origine della pagina nel browser.</span><span class="sxs-lookup"><span data-stu-id="61c2e-209">View the page's source in the browser.</span></span> <span data-ttu-id="61c2e-210">L'origine del browser Mostra molti attributi della maschera `data-val` (per la convalida dei dati).</span><span class="sxs-lookup"><span data-stu-id="61c2e-210">The browser source shows many attributes of the form `data-val` (for data validation).</span></span> <span data-ttu-id="61c2e-211">Quando la convalida del client e JavaScript non intrusivo è abilitato, i campi di input con una regola di convalida client contengono il `data-val="true"` attributo per attivare la convalida del client non intrusiva.</span><span class="sxs-lookup"><span data-stu-id="61c2e-211">When client validation and unobtrusive JavaScript is enabled, input fields with a client-validation rule contain the `data-val="true"` attribute to trigger unobtrusive client validation.</span></span> <span data-ttu-id="61c2e-212">Ad esempio, il `City` campo nel modello è decorata con il [necessari](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attributo, che comporta il codice HTML mostrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="61c2e-212">For example, the `City` field in the model was decorated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute, which results in the HTML shown in the following example:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

<span data-ttu-id="61c2e-213">Per ogni regola di convalida client, viene aggiunto un attributo che ha il formato `data-val-rulename="message"`.</span><span class="sxs-lookup"><span data-stu-id="61c2e-213">For each client-validation rule, an attribute is added that has the form `data-val-rulename="message"`.</span></span> <span data-ttu-id="61c2e-214">Utilizzando il `City` campo, ad esempio illustrato in precedenza, la regola di convalida client richiesta genera il `data-val-required` attributo e il messaggio &quot;città il campo è obbligatorio&quot;.</span><span class="sxs-lookup"><span data-stu-id="61c2e-214">Using the `City` field example shown earlier, the required client-validation rule generates the `data-val-required` attribute and the message &quot;The City field is required&quot;.</span></span> <span data-ttu-id="61c2e-215">Eseguire l'applicazione, modificare uno degli utenti e deselezionare il `City` campo.</span><span class="sxs-lookup"><span data-stu-id="61c2e-215">Run the application, edit one of the users, and clear the `City` field.</span></span> <span data-ttu-id="61c2e-216">Quando si preme tab fuori dal campo, viene visualizzato un messaggio di errore di convalida lato client.</span><span class="sxs-lookup"><span data-stu-id="61c2e-216">When you tab out of the field, you see a client-side validation error message.</span></span>

![Città richiesto](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

<span data-ttu-id="61c2e-218">Analogamente, per ogni parametro nella regola di convalida client, viene aggiunto un attributo che ha il formato `data-val-rulename-paramname=paramvalue`.</span><span class="sxs-lookup"><span data-stu-id="61c2e-218">Similarly, for each parameter in the client-validation rule, an attribute is added that has the form `data-val-rulename-paramname=paramvalue`.</span></span> <span data-ttu-id="61c2e-219">Ad esempio, il `FirstName` proprietà viene annotata con il [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) degli attributi e specifica una lunghezza minima di 3 e la lunghezza massima di 8.</span><span class="sxs-lookup"><span data-stu-id="61c2e-219">For example, the `FirstName` property is annotated with the [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute and specifies a minimum length of 3 and a maximum length of 8.</span></span> <span data-ttu-id="61c2e-220">La regola di convalida di dati denominata `length` è il nome del parametro `max` e il valore del parametro 8.</span><span class="sxs-lookup"><span data-stu-id="61c2e-220">The data validation rule named `length` has the parameter name `max` and the parameter value 8.</span></span> <span data-ttu-id="61c2e-221">Di seguito è riportato il codice HTML generato per il `FirstName` campo quando si modifica uno degli utenti:</span><span class="sxs-lookup"><span data-stu-id="61c2e-221">The following shows the HTML that is generated for the `FirstName` field when you edit one of the users:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

<span data-ttu-id="61c2e-222">Per ulteriori informazioni sulla convalida del client non intrusiva, vedere la voce [non intrusivo la convalida del Client in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) nel blog di Brad Wilson.</span><span class="sxs-lookup"><span data-stu-id="61c2e-222">For more information about unobtrusive client validation, see the entry [Unobtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="61c2e-223">Nella versione Beta di ASP.NET MVC 3, è talvolta necessario inviare il form per avviare la convalida lato client.</span><span class="sxs-lookup"><span data-stu-id="61c2e-223">In ASP.NET MVC 3 Beta, you sometimes need to submit the form in order to start client-side validation.</span></span> <span data-ttu-id="61c2e-224">Questo può essere modificato per la versione finale.</span><span class="sxs-lookup"><span data-stu-id="61c2e-224">This might be changed for the final release.</span></span>


## <a name="creating-the-create-view"></a><span data-ttu-id="61c2e-225">Creazione della visualizzazione di creazione</span><span class="sxs-lookup"><span data-stu-id="61c2e-225">Creating the Create View</span></span>

<span data-ttu-id="61c2e-226">Il passaggio successivo consiste nell'aggiungere un `Create` metodo di azione e visualizzazione per consentire all'utente di creare un nuovo utente.</span><span class="sxs-lookup"><span data-stu-id="61c2e-226">The next step is to add a `Create` action method and view in order to enable the user to create a new user.</span></span> <span data-ttu-id="61c2e-227">Aggiungere il seguente `Create` metodo al controller home:</span><span class="sxs-lookup"><span data-stu-id="61c2e-227">Add the following `Create` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

<span data-ttu-id="61c2e-228">Aggiungere una visualizzazione, come illustrato nei passaggi precedenti, ma impostare **visualizzare il contenuto** a **crea**.</span><span class="sxs-lookup"><span data-stu-id="61c2e-228">Add a view as in the previous steps, but set **View content** to **Create**.</span></span>

![Create View](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

<span data-ttu-id="61c2e-230">Eseguire l'applicazione, selezionare il **crea** collegare e aggiungere un nuovo utente.</span><span class="sxs-lookup"><span data-stu-id="61c2e-230">Run the application, select the **Create** link, and add a new user.</span></span> <span data-ttu-id="61c2e-231">Il `Create` metodo automaticamente sfrutta la convalida lato client e lato server.</span><span class="sxs-lookup"><span data-stu-id="61c2e-231">The `Create` method automatically takes advantage of client-side and server-side validation.</span></span> <span data-ttu-id="61c2e-232">Provare a immettere un nome utente che contiene gli spazi vuoti, ad esempio &quot;X Ben&quot;.</span><span class="sxs-lookup"><span data-stu-id="61c2e-232">Try to enter a user name that contains white space, such as &quot;Ben X&quot;.</span></span> <span data-ttu-id="61c2e-233">Quando esce dal campo nome utente, un errore di convalida sul lato client (`White space is not allowed`) viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="61c2e-233">When you tab out of the user name field, a client-side validation error (`White space is not allowed`) is displayed.</span></span>

## <a name="add-the-delete-method"></a><span data-ttu-id="61c2e-234">Aggiungere il metodo Delete</span><span class="sxs-lookup"><span data-stu-id="61c2e-234">Add the Delete method</span></span>

<span data-ttu-id="61c2e-235">Per completare l'esercitazione, aggiungere il seguente `Delete` metodo al controller home:</span><span class="sxs-lookup"><span data-stu-id="61c2e-235">To complete the tutorial, add the following `Delete` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

<span data-ttu-id="61c2e-236">Aggiungere un `Delete` vista come i passaggi precedenti, l'impostazione **visualizzare il contenuto** a **eliminare**.</span><span class="sxs-lookup"><span data-stu-id="61c2e-236">Add a `Delete` view as in the previous steps, setting **View content** to **Delete**.</span></span>

![Elimina vista](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

<span data-ttu-id="61c2e-238">È ora disponibile un'applicazione ASP.NET MVC 3 semplice ma completamente funzionale con la convalida.</span><span class="sxs-lookup"><span data-stu-id="61c2e-238">You now have a simple but fully functional ASP.NET MVC 3 application with validation.</span></span>
