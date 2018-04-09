---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Aggiunta della convalida per il modello | Documenti Microsoft
author: shanselman
description: Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Creare un'applicazione web semplice la lettura e scrittura da un database.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 78dd6bdd81fcb51a3a21a8f1ee12b4b2bfc37db5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="adding-validation-to-the-model"></a><span data-ttu-id="1a708-104">Aggiunta della convalida per il modello</span><span class="sxs-lookup"><span data-stu-id="1a708-104">Adding Validation to the Model</span></span>
====================
<span data-ttu-id="1a708-105">da [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="1a708-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="1a708-106">Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1a708-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="1a708-107">Si creerà un'applicazione web semplice la lettura e scrittura da un database.</span><span class="sxs-lookup"><span data-stu-id="1a708-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="1a708-108">Visitare il [area risorse di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.</span><span class="sxs-lookup"><span data-stu-id="1a708-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="1a708-109">In questa sezione verrà per implementare il supporto necessario per abilitare la convalida dell'input interno dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="1a708-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="1a708-110">Si farà in modo che il contenuto del database è sempre corretto e fornire utili messaggi di errore per gli utenti finali quando immettere dati filmato che non sono validi e riprovare.</span><span class="sxs-lookup"><span data-stu-id="1a708-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="1a708-111">Inizieremo aggiungendo un po' logica di convalida per la classe di film.</span><span class="sxs-lookup"><span data-stu-id="1a708-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="1a708-112">Fare clic con il pulsante destro sulla cartella del modello e selezionare Aggiungi classe.</span><span class="sxs-lookup"><span data-stu-id="1a708-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="1a708-113">Nome classe del film.</span><span class="sxs-lookup"><span data-stu-id="1a708-113">Name your class Movie.</span></span>

<span data-ttu-id="1a708-114">Quando abbiamo creato il modello di entità film in precedenza, l'IDE creata una classe di film.</span><span class="sxs-lookup"><span data-stu-id="1a708-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="1a708-115">In effetti, parte della classe film può essere in un file e una parte in un altro.</span><span class="sxs-lookup"><span data-stu-id="1a708-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="1a708-116">Si tratta di una classe parziale.</span><span class="sxs-lookup"><span data-stu-id="1a708-116">This is called a Partial Class.</span></span> <span data-ttu-id="1a708-117">Si intende estendere la classe di film da un altro file.</span><span class="sxs-lookup"><span data-stu-id="1a708-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="1a708-118">Verrà creata una classe parziale filmato che punta a una "classe buddy" con alcuni attributi che genererà l'hint per la convalida per il sistema.</span><span class="sxs-lookup"><span data-stu-id="1a708-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="1a708-119">Verrà contrassegnare il titolo e il prezzo come richiesto e insistere anche che il prezzo deve essere all'interno di un determinato intervallo.</span><span class="sxs-lookup"><span data-stu-id="1a708-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="1a708-120">Fare clic sulla cartella modelli e selezionare Aggiungi classe.</span><span class="sxs-lookup"><span data-stu-id="1a708-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="1a708-121">Nome classe del filmato e fare clic sul pulsante OK.</span><span class="sxs-lookup"><span data-stu-id="1a708-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="1a708-122">Di seguito è presentato il nostro parziale film classe.</span><span class="sxs-lookup"><span data-stu-id="1a708-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="1a708-123">Eseguire nuovamente l'applicazione e provare a immettere un filmato con un prezzo maggiori di 100.</span><span class="sxs-lookup"><span data-stu-id="1a708-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="1a708-124">Dopo avere inviato il form viene visualizzato un errore.</span><span class="sxs-lookup"><span data-stu-id="1a708-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="1a708-125">L'errore viene intercettato sul lato server e si verifica dopo l'invio del Form.</span><span class="sxs-lookup"><span data-stu-id="1a708-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="1a708-126">Si noti come helper HTML incorporato di ASP.NET MVC sono stati abbastanza per visualizzare il messaggio di errore e gestire i valori per noi all'interno gli elementi della casella di testo:</span><span class="sxs-lookup"><span data-stu-id="1a708-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="1a708-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1a708-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="1a708-128">Questo è particolarmente efficace, ma sarebbe utile se è stato possibile indicare all'utente sul lato client, immediatamente, prima che il server ottiene coinvolto.</span><span class="sxs-lookup"><span data-stu-id="1a708-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="1a708-129">Consente di abilitare una convalida sul lato client con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1a708-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="1a708-130">Aggiunta della convalida lato Client</span><span class="sxs-lookup"><span data-stu-id="1a708-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="1a708-131">Poiché la classe di film dispone già di alcuni attributi di convalida, è necessario solo aggiungere alcuni file JavaScript per il modello di visualizzazione Create.aspx e aggiungere una riga di codice per abilitare la convalida lato client da eseguire sul posto.</span><span class="sxs-lookup"><span data-stu-id="1a708-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="1a708-132">Dall'interno VWD passare la cartella viste o film e aprire Create.aspx.</span><span class="sxs-lookup"><span data-stu-id="1a708-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="1a708-133">Aprire la cartella di script in Esplora soluzioni e trascinare gli script seguenti tre all'interno di &lt;head&gt; tag.</span><span class="sxs-lookup"><span data-stu-id="1a708-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="1a708-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="1a708-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="1a708-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="1a708-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="1a708-136">Si desidera questi file di script vengono visualizzati nell'ordine indicato.</span><span class="sxs-lookup"><span data-stu-id="1a708-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="1a708-137">Inoltre, aggiungere quest'unica riga sopra il Html.BeginForm:</span><span class="sxs-lookup"><span data-stu-id="1a708-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="1a708-138">Di seguito è riportato il codice mostrato all'interno dell'IDE.</span><span class="sxs-lookup"><span data-stu-id="1a708-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="1a708-139">[![Filmati - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="1a708-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="1a708-140">Eseguire l'applicazione e visitare di nuovo /Movies/Create e fare clic su Crea senza dover immettere tutti i dati.</span><span class="sxs-lookup"><span data-stu-id="1a708-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="1a708-141">I messaggi di errore vengono visualizzati immediatamente senza la pagina flash che è associato l'invio di dati completamente al server.</span><span class="sxs-lookup"><span data-stu-id="1a708-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="1a708-142">Si tratta poiché ASP.NET MVC ora convalida l'input sia il client (tramite JavaScript) e nel server.</span><span class="sxs-lookup"><span data-stu-id="1a708-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="1a708-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="1a708-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="1a708-144">Questa ricerca buona!</span><span class="sxs-lookup"><span data-stu-id="1a708-144">This is looking good!</span></span> <span data-ttu-id="1a708-145">Aggiungere ora una colonna aggiuntiva per il database.</span><span class="sxs-lookup"><span data-stu-id="1a708-145">Let's now add one additional column to the database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1a708-146">[Precedente](getting-started-with-mvc-part6.md)
> [Successivo](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="1a708-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
