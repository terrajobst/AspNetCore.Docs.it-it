---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: L'integrazione con l'associazione di modelli e web form JQuery UI Datepicker | Documenti Microsoft
author: tfitzmac
description: Questa serie di esercitazioni illustra gli aspetti di base dell'utilizzo di associazione del modello con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 126262b440f3e914a7fac3f0b7eeadb4f648d2bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887912"
---
<a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a><span data-ttu-id="39ddf-104">L'integrazione di JQuery UI Datepicker con l'associazione di modelli e web form</span><span class="sxs-lookup"><span data-stu-id="39ddf-104">Integrating JQuery UI Datepicker with model binding and web forms</span></span>
====================
<span data-ttu-id="39ddf-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="39ddf-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="39ddf-106">Questa serie di esercitazioni illustra gli aspetti di base dell'utilizzo di associazione del modello con un progetto di Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="39ddf-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="39ddf-107">Associazione di modelli consente l'interazione dei dati più semplice rispetto a gestione dati di oggetti di origine (ad esempio ObjectDataSource o SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="39ddf-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="39ddf-108">Questa serie inizia con materiale introduttivo e sposta concetti più avanzati nelle esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="39ddf-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="39ddf-109">In questa esercitazione viene illustrato come aggiungere la UI JQuery [Datepicker widget](http://jqueryui.com/datepicker/) in un Web Form e l'utilizzo del modello di associazione per aggiornare il database con il valore selezionato.</span><span class="sxs-lookup"><span data-stu-id="39ddf-109">This tutorial shows how to add the JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) to a Web Form, and use model binding to update the database with the selected value.</span></span>
> 
> <span data-ttu-id="39ddf-110">Questa esercitazione si basa sul progetto creato nel [prima](retrieving-data.md) e [secondo](updating-deleting-and-creating-data.md) parti della serie.</span><span class="sxs-lookup"><span data-stu-id="39ddf-110">This tutorial builds on the project created in the [first](retrieving-data.md) and [second](updating-deleting-and-creating-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="39ddf-111">È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in c# o VB.</span><span class="sxs-lookup"><span data-stu-id="39ddf-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="39ddf-112">Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="39ddf-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="39ddf-113">Usa il modello di Visual Studio 2012, è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="39ddf-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="39ddf-114">Occorre invece compilare</span><span class="sxs-lookup"><span data-stu-id="39ddf-114">What you'll build</span></span>

<span data-ttu-id="39ddf-115">In questa esercitazione, effettuare le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="39ddf-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="39ddf-116">Aggiungere una proprietà al modello per registrare la data di registrazione dello studente</span><span class="sxs-lookup"><span data-stu-id="39ddf-116">Add a property to your model to record the student's enrollment date</span></span>
2. <span data-ttu-id="39ddf-117">Consentire all'utente di selezionare la data di registrazione utilizzando il widget JQuery UI Datepicker</span><span class="sxs-lookup"><span data-stu-id="39ddf-117">Enable the user to select the enrollment date using the JQuery UI Datepicker widget</span></span>
3. <span data-ttu-id="39ddf-118">Applicare le regole di convalida per la data di registrazione</span><span class="sxs-lookup"><span data-stu-id="39ddf-118">Enforce validation rules for the enrollment date</span></span>

<span data-ttu-id="39ddf-119">Il widget JQuery UI Datepicker consente agli utenti di selezionare con facilità una data dal calendario che viene visualizzato quando l'utente interagisce con il campo.</span><span class="sxs-lookup"><span data-stu-id="39ddf-119">The JQuery UI Datepicker widget enables users to easily select a date from a calendar that pops up when the user interacts with the field.</span></span> <span data-ttu-id="39ddf-120">Utilizzo di questo widget può essere più conveniente per gli utenti di manualmente digitando una data.</span><span class="sxs-lookup"><span data-stu-id="39ddf-120">Using this widget can be more convenient for users than manually typing a date.</span></span> <span data-ttu-id="39ddf-121">Integrazione di widget Datepicker in una pagina che utilizza l'associazione di modelli per le operazioni di dati richiede solo una piccola quantità di lavoro aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="39ddf-121">Integrating the Datepicker widget into a page that uses model binding for data operations requires only a small amount of additional work.</span></span>

## <a name="add-a-new-property-to-the-model"></a><span data-ttu-id="39ddf-122">Aggiungere una nuova proprietà al modello</span><span class="sxs-lookup"><span data-stu-id="39ddf-122">Add a new property to the model</span></span>

<span data-ttu-id="39ddf-123">In primo luogo, si aggiungerà un **Datetime** proprietà per il vostro studente del modello ed eseguire la migrazione di tale modifica al database.</span><span class="sxs-lookup"><span data-stu-id="39ddf-123">First, you will add a **Datetime** property to your Student model and migrate that change to the database.</span></span> <span data-ttu-id="39ddf-124">Aprire **UniversityModels.cs**e aggiungere il codice evidenziato per il modello di studenti.</span><span class="sxs-lookup"><span data-stu-id="39ddf-124">Open **UniversityModels.cs**, and add the highlighted code to the Student model.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

<span data-ttu-id="39ddf-125">Il **RangeAttribute** è incluso per applicare le regole di convalida per la proprietà.</span><span class="sxs-lookup"><span data-stu-id="39ddf-125">The **RangeAttribute** is included to enforce validation rules for the property.</span></span> <span data-ttu-id="39ddf-126">Per questa esercitazione, si suppone che Contoso University fondata sul 1 ° gennaio 2013 e pertanto non sono valide le date di registrazione precedente.</span><span class="sxs-lookup"><span data-stu-id="39ddf-126">For this tutorial, we will assume that Contoso University was founded on January 1st, 2013 and therefore earlier enrollment dates are not valid.</span></span>

<span data-ttu-id="39ddf-127">Nella finestra di gestione dei pacchetti, aggiungere una migrazione eseguendo il comando **AddEnrollmentDate migrazione aggiungere**.</span><span class="sxs-lookup"><span data-stu-id="39ddf-127">In the Package Management window, add a migration by running the command **add-migration AddEnrollmentDate**.</span></span> <span data-ttu-id="39ddf-128">Si noti che il codice di migrazione aggiunge la nuova colonna Datetime nella tabella di studenti.</span><span class="sxs-lookup"><span data-stu-id="39ddf-128">Notice that the migration code adds the new Datetime column to the Student table.</span></span> <span data-ttu-id="39ddf-129">Per trovare il valore specificato è il RangeAttribute, aggiungere un valore predefinito per la nuova colonna, come illustrato nel seguente codice evidenziato.</span><span class="sxs-lookup"><span data-stu-id="39ddf-129">To match the value you specified in the RangeAttribute, add a default value for the new column, as shown in the highlighted code below.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

<span data-ttu-id="39ddf-130">Salvare la modifica del file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="39ddf-130">Save your change to the migration file.</span></span>

<span data-ttu-id="39ddf-131">Non è necessario inizializzare nuovamente i dati.</span><span class="sxs-lookup"><span data-stu-id="39ddf-131">You do not need to seed the data again.</span></span> <span data-ttu-id="39ddf-132">Pertanto, aprire **Configuration.cs** nella cartella migrazioni e rimuovere o impostare come commento il codice il **valore di inizializzazione** (metodo).</span><span class="sxs-lookup"><span data-stu-id="39ddf-132">Therefore, open **Configuration.cs** in the Migrations folder and remove or comment out the code in the **Seed** method.</span></span> <span data-ttu-id="39ddf-133">Salvare e chiudere il file.</span><span class="sxs-lookup"><span data-stu-id="39ddf-133">Save and close the file.</span></span>

<span data-ttu-id="39ddf-134">A questo punto, eseguire il comando **aggiornamento database**.</span><span class="sxs-lookup"><span data-stu-id="39ddf-134">Now, run the command **update-database**.</span></span> <span data-ttu-id="39ddf-135">Si noti che la colonna è ora presente nel database e di tutti i record esistenti il valore predefinito per EnrollmentDate.</span><span class="sxs-lookup"><span data-stu-id="39ddf-135">Notice that the column now exists in the database and all of the existing records have the default value for EnrollmentDate.</span></span>

## <a name="add-dynamic-controls-for-enrollment-date"></a><span data-ttu-id="39ddf-136">Aggiungere i controlli dinamici per la data di registrazione</span><span class="sxs-lookup"><span data-stu-id="39ddf-136">Add dynamic controls for enrollment date</span></span>

<span data-ttu-id="39ddf-137">A questo punto si aggiungerà i controlli per visualizzare e modificare la data di registrazione.</span><span class="sxs-lookup"><span data-stu-id="39ddf-137">You will now add controls for displaying and editing the enrollment date.</span></span> <span data-ttu-id="39ddf-138">A questo punto, il valore viene modificato tramite una casella di testo.</span><span class="sxs-lookup"><span data-stu-id="39ddf-138">At this point, the value is edited through a text box.</span></span> <span data-ttu-id="39ddf-139">Più avanti nell'esercitazione si modificherà la casella di testo per il widget JQuery Datepicker.</span><span class="sxs-lookup"><span data-stu-id="39ddf-139">Later in the tutorial, you will change the text box to the JQuery Datepicker widget.</span></span>

<span data-ttu-id="39ddf-140">Innanzitutto, è importante notare che è necessario apportare qualsiasi modifica apportata al **AddStudent.aspx** file.</span><span class="sxs-lookup"><span data-stu-id="39ddf-140">First, it is important to note that you do not need to make any change to the **AddStudent.aspx** file.</span></span> <span data-ttu-id="39ddf-141">Il controllo DynamicEntity renderà automaticamente la nuova proprietà.</span><span class="sxs-lookup"><span data-stu-id="39ddf-141">The DynamicEntity control will automatically render the new property.</span></span>

<span data-ttu-id="39ddf-142">Aprire **Students.aspx**e aggiungere il codice evidenziato di seguito.</span><span class="sxs-lookup"><span data-stu-id="39ddf-142">Open **Students.aspx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

<span data-ttu-id="39ddf-143">Eseguire l'applicazione e si noti che è possibile impostare il valore della data di registrazione digitando una data.</span><span class="sxs-lookup"><span data-stu-id="39ddf-143">Run the application and notice that you can set the value of the enrollment date by typing a date.</span></span> <span data-ttu-id="39ddf-144">Quando si aggiunge un nuovo studente:</span><span class="sxs-lookup"><span data-stu-id="39ddf-144">When adding a new student:</span></span>

![impostare la data](integrating-jquery-ui/_static/image1.png)

<span data-ttu-id="39ddf-146">In alternativa, la modifica di un valore esistente:</span><span class="sxs-lookup"><span data-stu-id="39ddf-146">Or, editing an existing value:</span></span>

![Data modifica](integrating-jquery-ui/_static/image2.png)

<span data-ttu-id="39ddf-148">Digitare la data di funzioni, ma potrebbe non essere si desidera fornire l'esperienza del cliente.</span><span class="sxs-lookup"><span data-stu-id="39ddf-148">Typing the date works, but it might not be the customer experience you wish to provide.</span></span> <span data-ttu-id="39ddf-149">Nella sezione successiva, verrà abilitata la selezione di una data tramite un calendario.</span><span class="sxs-lookup"><span data-stu-id="39ddf-149">In the next section, you will enable selecting a date through a calendar.</span></span>

## <a name="install-nuget-package-to-work-with-jquery-ui"></a><span data-ttu-id="39ddf-150">Installare il pacchetto NuGet per funzionare con JQuery UI</span><span class="sxs-lookup"><span data-stu-id="39ddf-150">Install NuGet package to work with JQuery UI</span></span>

<span data-ttu-id="39ddf-151">Il **dell'interfaccia utente succo** pacchetto NuGet consente una facile integrazione dei widget JQuery UI nell'applicazione web.</span><span class="sxs-lookup"><span data-stu-id="39ddf-151">The **Juice UI** NuGet package enables easy integration of the JQuery UI widgets into your web application.</span></span> <span data-ttu-id="39ddf-152">Per utilizzare questo pacchetto, installarlo tramite NuGet.</span><span class="sxs-lookup"><span data-stu-id="39ddf-152">To use this package, install it through NuGet.</span></span>

![aggiunta dell'interfaccia utente di succo di](integrating-jquery-ui/_static/image3.png)

<span data-ttu-id="39ddf-154">La versione dell'interfaccia utente di succo installato sia in conflitto con la versione di JQuery nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="39ddf-154">The version of Juice UI that you install may conflict with the version of JQuery in your application.</span></span> <span data-ttu-id="39ddf-155">Prima di procedere con questa esercitazione, provare a eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="39ddf-155">Before proceeding with this tutorial, try running your application.</span></span> <span data-ttu-id="39ddf-156">Se si verifica un errore di JavaScript, è necessario risolvere le differenze tra la versione di JQuery.</span><span class="sxs-lookup"><span data-stu-id="39ddf-156">If you encounter a JavaScript error, you need to reconcile the JQuery version.</span></span> <span data-ttu-id="39ddf-157">È possibile aggiungere la versione prevista di JQuery alla cartella degli script (versione 1.8.2 al momento della stesura di questa esercitazione) o in Site. master, specificare il percorso al file JQuery.</span><span class="sxs-lookup"><span data-stu-id="39ddf-157">You can either add the expected version of JQuery to your Scripts folder (version 1.8.2 at time of writing this tutorial), or in Site.master specify the path to the JQuery file.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a><span data-ttu-id="39ddf-158">Personalizzare il modello di data/ora per includere i widget Datepicker</span><span class="sxs-lookup"><span data-stu-id="39ddf-158">Customize DateTime template to include Datepicker widget</span></span>

<span data-ttu-id="39ddf-159">Il widget Datepicker verranno aggiunti al modello di dati dinamici per la modifica di un valore datetime.</span><span class="sxs-lookup"><span data-stu-id="39ddf-159">You will add the Datepicker widget to the dynamic data template for editing a datetime value.</span></span> <span data-ttu-id="39ddf-160">Aggiungendo il widget per il modello, ne viene automaticamente eseguito il rendering nel formato per l'aggiunta di un nuovo studente e nella visualizzazione griglia per studenti di modifica.</span><span class="sxs-lookup"><span data-stu-id="39ddf-160">By adding the widget to the template, it is automatically rendered in both the form for adding a new student and in the grid view for editing students.</span></span> <span data-ttu-id="39ddf-161">Aprire **DateTime\_Edit. ascx**e aggiungere il codice evidenziato di seguito.</span><span class="sxs-lookup"><span data-stu-id="39ddf-161">Open **DateTime\_Edit.ascx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

<span data-ttu-id="39ddf-162">Nel file code-behind, si imposteranno le date minime e massime per la selezione data.</span><span class="sxs-lookup"><span data-stu-id="39ddf-162">In the code-behind file, you will set the minimum and maximum dates for the DatePicker.</span></span> <span data-ttu-id="39ddf-163">Impostando questi valori, si impedirà agli utenti di spostarsi tra le date non valide.</span><span class="sxs-lookup"><span data-stu-id="39ddf-163">By setting these values, you will prevent users from navigating to invalid dates.</span></span> <span data-ttu-id="39ddf-164">Si recupererà i valori minimo e massimi dal **RangeAttribute** sulla proprietà DateTime, se disponibile.</span><span class="sxs-lookup"><span data-stu-id="39ddf-164">You will retrieve the minimum and maximum values from the **RangeAttribute** on the DateTime property, if one is provided.</span></span> <span data-ttu-id="39ddf-165">Aprire **DateTime\_Edit.ascx.cs**e aggiungere il codice evidenziato di seguito alla pagina\_metodo Load.</span><span class="sxs-lookup"><span data-stu-id="39ddf-165">Open **DateTime\_Edit.ascx.cs**, and add the following highlighted code to the Page\_Load method.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

<span data-ttu-id="39ddf-166">Eseguire l'applicazione web e passare alla pagina AddStudent.</span><span class="sxs-lookup"><span data-stu-id="39ddf-166">Run the web application and navigate to the AddStudent page.</span></span> <span data-ttu-id="39ddf-167">Specificare i valori per i campi e notare che, quando si fa clic sulla casella di testo per la data di registrazione, viene visualizzato il calendario.</span><span class="sxs-lookup"><span data-stu-id="39ddf-167">Provide values for the fields and notice that when you click on the text box for Enrollment Date, the calendar is displayed.</span></span>

![selezione data](integrating-jquery-ui/_static/image4.png)

<span data-ttu-id="39ddf-169">Selezionare una data e fare clic su **inserire**.</span><span class="sxs-lookup"><span data-stu-id="39ddf-169">Pick a date, and click **Insert**.</span></span> <span data-ttu-id="39ddf-170">Il RangeAttribute applica la convalida sul server.</span><span class="sxs-lookup"><span data-stu-id="39ddf-170">The RangeAttribute enforces validation on the server.</span></span> <span data-ttu-id="39ddf-171">Impostando la proprietà minDate sul Datepicker, è inoltre applicare la convalida sul client.</span><span class="sxs-lookup"><span data-stu-id="39ddf-171">By setting the minDate property on the Datepicker, you also apply validation on the client.</span></span> <span data-ttu-id="39ddf-172">Il calendario non consentire all'utente di passare a una data precedente il valore della proprietà minDate.</span><span class="sxs-lookup"><span data-stu-id="39ddf-172">The calendar does not let the user navigate to a date prior to the value of minDate.</span></span>

<span data-ttu-id="39ddf-173">Quando si modifica un record nella visualizzazione griglia, viene visualizzato anche il calendario.</span><span class="sxs-lookup"><span data-stu-id="39ddf-173">When you edit a record in the grid view, the calendar is also displayed.</span></span>

![DatePicker in GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a><span data-ttu-id="39ddf-175">Conclusione</span><span class="sxs-lookup"><span data-stu-id="39ddf-175">Conclusion</span></span>

<span data-ttu-id="39ddf-176">In questa esercitazione è stato descritto come incorporare un widget di JQuery in un web form che utilizza l'associazione del modello.</span><span class="sxs-lookup"><span data-stu-id="39ddf-176">In this tutorial, you learned how to incorporate a JQuery widget into a web form that uses model binding.</span></span>

<span data-ttu-id="39ddf-177">Nella prossima [esercitazione](using-query-string-values-to-retrieve-data.md), si utilizzerà un valore di stringa di query quando la selezione di dati.</span><span class="sxs-lookup"><span data-stu-id="39ddf-177">In the next [tutorial](using-query-string-values-to-retrieve-data.md), you will use a query string value when selecting data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="39ddf-178">[Precedente](sorting-paging-and-filtering-data.md)
> [Successivo](using-query-string-values-to-retrieve-data.md)</span><span class="sxs-lookup"><span data-stu-id="39ddf-178">[Previous](sorting-paging-and-filtering-data.md)
[Next](using-query-string-values-to-retrieve-data.md)</span></span>
