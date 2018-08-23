---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Aggiunta della convalida al modello | Microsoft Docs
author: shanselman
description: Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Creare un'applicazione web semplice che legge e scrive da un database.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 4c7867587ba0610f0f1c23d9a0b9fbdc4040de7c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827502"
---
<a name="adding-validation-to-the-model"></a><span data-ttu-id="8e8af-104">Aggiunta della convalida al modello</span><span class="sxs-lookup"><span data-stu-id="8e8af-104">Adding Validation to the Model</span></span>
====================
<span data-ttu-id="8e8af-105">da [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="8e8af-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="8e8af-106">Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8e8af-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="8e8af-107">Si creerà una semplice applicazione web che legge e scrive da un database.</span><span class="sxs-lookup"><span data-stu-id="8e8af-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="8e8af-108">Visitare il [centro di formazione di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.</span><span class="sxs-lookup"><span data-stu-id="8e8af-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="8e8af-109">In questa sezione si intende implementare il supporto necessario per abilitare la convalida dell'input all'interno dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8e8af-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="8e8af-110">Si farà in modo che il contenuto del database sia sempre corretto e fornire messaggi di errore utili agli utenti finali quando si prova a immettere dati dei film che non è validi.</span><span class="sxs-lookup"><span data-stu-id="8e8af-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="8e8af-111">Inizieremo mediante l'aggiunta di un piccolo per la logica di convalida per la classe di film.</span><span class="sxs-lookup"><span data-stu-id="8e8af-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="8e8af-112">Fare clic con il pulsante destro sulla cartella del modello e selezionare Add Class.</span><span class="sxs-lookup"><span data-stu-id="8e8af-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="8e8af-113">Assegnare un nome classe del film.</span><span class="sxs-lookup"><span data-stu-id="8e8af-113">Name your class Movie.</span></span>

<span data-ttu-id="8e8af-114">Quando abbiamo creato il modello di entità film in precedenza, l'IDE creato una classe di film.</span><span class="sxs-lookup"><span data-stu-id="8e8af-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="8e8af-115">Infatti, può essere parte della classe di film in un file e parte in un altro.</span><span class="sxs-lookup"><span data-stu-id="8e8af-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="8e8af-116">Si tratta di una classe parziale.</span><span class="sxs-lookup"><span data-stu-id="8e8af-116">This is called a Partial Class.</span></span> <span data-ttu-id="8e8af-117">Dobbiamo estende la classe di film da un altro file.</span><span class="sxs-lookup"><span data-stu-id="8e8af-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="8e8af-118">Si creerà una classe parziale film che punta a una "classe buddy" con alcuni attributi contenenti hint per la convalida al sistema.</span><span class="sxs-lookup"><span data-stu-id="8e8af-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="8e8af-119">Verranno contrassegnare il titolo e il prezzo come obbligatorio e anche insistere che il prezzo di essere all'interno di un determinato intervallo.</span><span class="sxs-lookup"><span data-stu-id="8e8af-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="8e8af-120">Fare clic con il pulsante destro della cartella Models e scegliere Aggiungi classe.</span><span class="sxs-lookup"><span data-stu-id="8e8af-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="8e8af-121">Denominare la classe film e fare clic sul pulsante OK.</span><span class="sxs-lookup"><span data-stu-id="8e8af-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="8e8af-122">Ecco l'aspetto di classe film parziale come.</span><span class="sxs-lookup"><span data-stu-id="8e8af-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="8e8af-123">Eseguire nuovamente l'applicazione e provare a immettere un film con un prezzo maggiori di 100.</span><span class="sxs-lookup"><span data-stu-id="8e8af-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="8e8af-124">Si otterrà un errore dopo aver inviato il form.</span><span class="sxs-lookup"><span data-stu-id="8e8af-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="8e8af-125">L'errore viene intercettato sul lato server e si verifica dopo l'invio del Form.</span><span class="sxs-lookup"><span data-stu-id="8e8af-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="8e8af-126">Si noti come helper HTML predefiniti di ASP.NET MVC sono stati in grado di visualizzare il messaggio di errore e gestire i valori per noi all'interno di elementi nella casella di testo:</span><span class="sxs-lookup"><span data-stu-id="8e8af-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="8e8af-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8e8af-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="8e8af-128">Questo è particolarmente efficace, ma sarebbe bello se potremmo l'utente viene informato sul lato client, immediatamente, prima che il server viene coinvolto.</span><span class="sxs-lookup"><span data-stu-id="8e8af-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="8e8af-129">È possibile abilitare alcune convalide sul lato client con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8e8af-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="8e8af-130">Aggiunta della convalida lato Client</span><span class="sxs-lookup"><span data-stu-id="8e8af-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="8e8af-131">Poiché la classe di film dispone già di alcuni attributi di convalida, è necessario solo aggiungere alcuni file JavaScript per il modello di vista di tornarci e aggiungere una riga di codice per abilitare la convalida lato client per l'implementazione.</span><span class="sxs-lookup"><span data-stu-id="8e8af-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="8e8af-132">Dall'interno VWD passare la cartella visualizzazioni/film e apriamo tornarci.</span><span class="sxs-lookup"><span data-stu-id="8e8af-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="8e8af-133">Aprire la cartella degli script in Esplora soluzioni e trascinare tre gli script seguenti all'interno di &lt;head&gt; tag.</span><span class="sxs-lookup"><span data-stu-id="8e8af-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="8e8af-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="8e8af-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="8e8af-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="8e8af-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="8e8af-136">Si desidera che questi file script vengono visualizzati nell'ordine indicato.</span><span class="sxs-lookup"><span data-stu-id="8e8af-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="8e8af-137">Inoltre, aggiungere quest'unica riga sopra il Html.BeginForm:</span><span class="sxs-lookup"><span data-stu-id="8e8af-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="8e8af-138">Ecco il codice illustrato all'interno dell'IDE.</span><span class="sxs-lookup"><span data-stu-id="8e8af-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="8e8af-139">[![Film - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="8e8af-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="8e8af-140">Eseguire l'applicazione e accedere di nuovo /Movies/Create e fare clic su Crea senza dover immettere tutti i dati.</span><span class="sxs-lookup"><span data-stu-id="8e8af-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="8e8af-141">I messaggi di errore vengono visualizzati immediatamente senza la pagina flash che viene associato l'invio di dati fino al server.</span><span class="sxs-lookup"><span data-stu-id="8e8af-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="8e8af-142">Si tratta poiché ASP.NET MVC è ora la convalida dell'input sia il client (tramite JavaScript) e nel server.</span><span class="sxs-lookup"><span data-stu-id="8e8af-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="8e8af-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="8e8af-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="8e8af-144">Ciò è bene</span><span class="sxs-lookup"><span data-stu-id="8e8af-144">This is looking good!</span></span> <span data-ttu-id="8e8af-145">Questo punto, aggiungere una colonna aggiuntiva al database.</span><span class="sxs-lookup"><span data-stu-id="8e8af-145">Let's now add one additional column to the database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8e8af-146">[Precedente](getting-started-with-mvc-part6.md)
> [Successivo](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="8e8af-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
