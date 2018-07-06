---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Creare un nuovo progetto ASP.NET MVC | Microsoft Docs
author: microsoft
description: Passaggio 1 viene illustrato come implementare la struttura dell'applicazione di NerdDinner base.
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 88ef850503725cc57c92a11952729b4bd2205a69
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835094"
---
<a name="create-a-new-aspnet-mvc-project"></a><span data-ttu-id="6fd23-103">Creare un nuovo progetto ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="6fd23-103">Create a New ASP.NET MVC Project</span></span>
====================
<span data-ttu-id="6fd23-104">da [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="6fd23-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="6fd23-105">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="6fd23-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="6fd23-106">Si tratta di passaggio 1 di una liberazione [esercitazione sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-dettaglio come compilare una piccola, ma completa, applicazione web con ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="6fd23-106">This is step 1 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="6fd23-107">Passaggio 1 viene illustrato come implementare la struttura dell'applicazione di NerdDinner base.</span><span class="sxs-lookup"><span data-stu-id="6fd23-107">Step 1 shows you how to put the basic NerdDinner application structure in place.</span></span>
> 
> <span data-ttu-id="6fd23-108">Se si usa ASP.NET MVC 3, è consigliabile seguire le [Guida introduttiva con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oppure [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.</span><span class="sxs-lookup"><span data-stu-id="6fd23-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-1-file-gtnew-project"></a><span data-ttu-id="6fd23-109">NerdDinner passaggio 1: File -&gt;nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="6fd23-109">NerdDinner Step 1: File-&gt;New Project</span></span>

<span data-ttu-id="6fd23-110">Inizieremo la nostra applicazione NerdDinner selezionando il **File -&gt;nuovo progetto** voce di menu all'interno di Visual Studio 2008 o il gratuito Visual Web Developer 2008 Express.</span><span class="sxs-lookup"><span data-stu-id="6fd23-110">We'll begin our NerdDinner application by selecting the **File-&gt;New Project** menu item within either Visual Studio 2008 or the free Visual Web Developer 2008 Express.</span></span>

<span data-ttu-id="6fd23-111">Verrà visualizzata la finestra di dialogo "Nuovo progetto".</span><span class="sxs-lookup"><span data-stu-id="6fd23-111">This will bring up the "New Project" dialog.</span></span> <span data-ttu-id="6fd23-112">Per creare una nuova applicazione MVC ASP.NET, si sarà selezionare il nodo "Web" sul lato sinistro della finestra di dialogo e quindi scegliere il modello di progetto "Applicazione Web ASP.NET MVC" a destra:</span><span class="sxs-lookup"><span data-stu-id="6fd23-112">To create a new ASP.NET MVC application, we'll select the "Web" node on the left-hand side of the dialog and then choose the "ASP.NET MVC Web Application" project template on the right:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image1.png)

<span data-ttu-id="6fd23-113">*Importante: Verificare che è stata scaricata e installata ASP.NET MVC - in caso contrario non verrà visualizzato nella finestra di dialogo Nuovo progetto. È possibile usare V2 del [installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) se è ancora installato ancora (ASP.NET MVC è disponibile all'interno di "piattaforma Web -&gt;Framework e runtime" sezione).*</span><span class="sxs-lookup"><span data-stu-id="6fd23-113">*Important: Make sure you've downloaded and installed ASP.NET MVC - otherwise it won't show up in the New Project dialog. You can use V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) if you haven't installed it yet (ASP.NET MVC is available within the "Web Platform-&gt;Frameworks and Runtimes" section).*</span></span>

<span data-ttu-id="6fd23-114">Sarà denominato nuovo progetto che si intende creare "NerdDinner" e quindi fare clic sul pulsante "ok" per crearlo.</span><span class="sxs-lookup"><span data-stu-id="6fd23-114">We'll name the new project we are going to create "NerdDinner" and then click the "ok" button to create it.</span></span>

<span data-ttu-id="6fd23-115">Quando si fa clic su "ok" Visual Studio verrà visualizzata una finestra di dialogo aggiuntiva che consente, facoltativamente, creare un progetto di unit test per la nuova applicazione.</span><span class="sxs-lookup"><span data-stu-id="6fd23-115">When we click "ok" Visual Studio will bring up an additional dialog that prompts us to optionally create a unit test project for the new application as well.</span></span> <span data-ttu-id="6fd23-116">Questo progetto di unit test ci permette di creare test automatizzati che consentono di verificare le funzionalità e il comportamento dell'applicazione (qualcosa si affronterà il modo in cui le cose da fare più avanti in questa esercitazione).</span><span class="sxs-lookup"><span data-stu-id="6fd23-116">This unit test project enables us to create automated tests that verify the functionality and behavior of our application (something we'll cover how to-do later in this tutorial).</span></span>

![](create-a-new-aspnet-mvc-project/_static/image2.png)

<span data-ttu-id="6fd23-117">L'elenco a discesa "Framework di Test" nella finestra di dialogo precedente viene popolato con tutti i disponibili ASP.NET MVC unit test di modelli di progetto installati nel computer.</span><span class="sxs-lookup"><span data-stu-id="6fd23-117">The "Test framework" dropdown in the above dialog is populated with all available ASP.NET MVC unit test project templates installed on the machine.</span></span> <span data-ttu-id="6fd23-118">Le versioni possono essere scaricate per MBUnit, NUnit e XUnit.</span><span class="sxs-lookup"><span data-stu-id="6fd23-118">Versions can be downloaded for NUnit, MBUnit, and XUnit.</span></span> <span data-ttu-id="6fd23-119">È supportato anche il framework di Unit Test di Visual Studio incorporato.</span><span class="sxs-lookup"><span data-stu-id="6fd23-119">The built-in Visual Studio Unit Test framework is also supported.</span></span>

<span data-ttu-id="6fd23-120">*Nota: Il Framework Unit Test di Visual Studio è disponibile solo con Visual Studio 2008 Professional e versioni successive. Se si usa Visual Studio 2008 Standard Edition o Visual Web Developer 2008 Express è necessario scaricare e installare le estensioni di NUnit, MBUnit o XUnit per ASP.NET MVC di questa finestra di dialogo da visualizzare. Se non ci sono qualsiasi framework di test installato, non verrà visualizzata la finestra di dialogo.*</span><span class="sxs-lookup"><span data-stu-id="6fd23-120">*Note: The Visual Studio Unit Test Framework is only available with Visual Studio 2008 Professional and higher versions. If you are using VS 2008 Standard Edition or Visual Web Developer 2008 Express you will need to download and install the NUnit, MBUnit or XUnit extensions for ASP.NET MVC in order for this dialog to be shown. The dialog will not display if there aren't any test frameworks installed.*</span></span>

<span data-ttu-id="6fd23-121">È possibile utilizzare quello predefinito "NerdDinner.Tests" per il progetto di test che viene creata e usare l'opzione di framework "Unit Test con Visual Studio".</span><span class="sxs-lookup"><span data-stu-id="6fd23-121">We'll use the default "NerdDinner.Tests" name for the test project we create, and use the "Visual Studio Unit Test" framework option.</span></span> <span data-ttu-id="6fd23-122">Quando si fa clic sul pulsante "ok" Visual Studio verrà creata una soluzione per noi con due progetti in essa - uno per la nostra applicazione web e uno per i test di unità:</span><span class="sxs-lookup"><span data-stu-id="6fd23-122">When we click the "ok" button Visual Studio will create a solution for us with two projects in it - one for our web application and one for our unit tests:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a><span data-ttu-id="6fd23-123">Esame della struttura di directory di NerdDinner</span><span class="sxs-lookup"><span data-stu-id="6fd23-123">Examining the NerdDinner directory structure</span></span>

<span data-ttu-id="6fd23-124">Quando si crea una nuova applicazione ASP.NET MVC con Visual Studio, lo aggiunge automaticamente un numero di file e directory al progetto:</span><span class="sxs-lookup"><span data-stu-id="6fd23-124">When you create a new ASP.NET MVC application with Visual Studio, it automatically adds a number of files and directories to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image4.png)

<span data-ttu-id="6fd23-125">I progetti ASP.NET MVC per impostazione predefinita sono sei directory di primo livello:</span><span class="sxs-lookup"><span data-stu-id="6fd23-125">ASP.NET MVC projects by default have six top-level directories:</span></span>

| <span data-ttu-id="6fd23-126">**Directory**</span><span class="sxs-lookup"><span data-stu-id="6fd23-126">**Directory**</span></span> | <span data-ttu-id="6fd23-127">**Scopo**</span><span class="sxs-lookup"><span data-stu-id="6fd23-127">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="6fd23-128">**/ Controller**</span><span class="sxs-lookup"><span data-stu-id="6fd23-128">**/Controllers**</span></span> | <span data-ttu-id="6fd23-129">Si inseriranno le classi Controller che gestiscono le richieste di URL</span><span class="sxs-lookup"><span data-stu-id="6fd23-129">Where you put Controller classes that handle URL requests</span></span> |
| <span data-ttu-id="6fd23-130">**O i modelli**</span><span class="sxs-lookup"><span data-stu-id="6fd23-130">**/Models**</span></span> | <span data-ttu-id="6fd23-131">Si inseriranno le classi che rappresentano e modificano i dati</span><span class="sxs-lookup"><span data-stu-id="6fd23-131">Where you put classes that represent and manipulate data</span></span> |
| <span data-ttu-id="6fd23-132">**/ Viste**</span><span class="sxs-lookup"><span data-stu-id="6fd23-132">**/Views**</span></span> | <span data-ttu-id="6fd23-133">In cui vengono salvati i file di modello di interfaccia utente che sono responsabili di output del rendering</span><span class="sxs-lookup"><span data-stu-id="6fd23-133">Where you put UI template files that are responsible for rendering output</span></span> |
| <span data-ttu-id="6fd23-134">**/ Script**</span><span class="sxs-lookup"><span data-stu-id="6fd23-134">**/Scripts**</span></span> | <span data-ttu-id="6fd23-135">Si inseriranno i file di libreria JavaScript e script (con estensione js)</span><span class="sxs-lookup"><span data-stu-id="6fd23-135">Where you put JavaScript library files and scripts (.js)</span></span> |
| <span data-ttu-id="6fd23-136">**/Content**</span><span class="sxs-lookup"><span data-stu-id="6fd23-136">**/Content**</span></span> | <span data-ttu-id="6fd23-137">Si inseriranno CSS e file di immagine e altri contenuti non non dinamica/JavaScript</span><span class="sxs-lookup"><span data-stu-id="6fd23-137">Where you put CSS and image files, and other non-dynamic/non-JavaScript content</span></span> |
| <span data-ttu-id="6fd23-138">**/App\_Data**</span><span class="sxs-lookup"><span data-stu-id="6fd23-138">**/App\_Data**</span></span> | <span data-ttu-id="6fd23-139">In cui si archiviano file di dati che si desidera lettura/scrittura.</span><span class="sxs-lookup"><span data-stu-id="6fd23-139">Where you store data files you want to read/write.</span></span> |

<span data-ttu-id="6fd23-140">ASP.NET MVC non richiede questa struttura.</span><span class="sxs-lookup"><span data-stu-id="6fd23-140">ASP.NET MVC does not require this structure.</span></span> <span data-ttu-id="6fd23-141">In effetti, gli sviluppatori che lavorano su applicazioni di grandi dimensioni in genere suddividerà l'applicazione backup tra più progetti per renderla più gestibile (ad esempio: classi di modello di dati spesso passare in un progetto di libreria di classi separata dall'applicazione web).</span><span class="sxs-lookup"><span data-stu-id="6fd23-141">In fact, developers working on large applications will typically partition the application up across multiple projects to make it more manageable (for example: data model classes often go in a separate class library project from the web application).</span></span> <span data-ttu-id="6fd23-142">La struttura del progetto predefinita, tuttavia, fornisce una convenzione di directory predefinito interessante che è possibile usare per mantenere pulito i problemi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="6fd23-142">The default project structure, however, does provide a nice default directory convention that we can use to keep our application concerns clean.</span></span>

<span data-ttu-id="6fd23-143">Quando si espande la directory /Controllers verrà individuato che Visual Studio ha aggiunto due classi controller, HomeController e AccountController, per impostazione predefinita al progetto:</span><span class="sxs-lookup"><span data-stu-id="6fd23-143">When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image5.png)

<span data-ttu-id="6fd23-144">Quando si espande la directory di modelli, è possibile trovare tre sottodirectory: avremo, /Account e /Shared – nonché modelli diversi all'interno di essi sono stati anche aggiunti file al progetto per impostazione predefinita:</span><span class="sxs-lookup"><span data-stu-id="6fd23-144">When we expand the /Views directory, we'll find three sub-directories – /Home, /Account and /Shared – as well as several template files within them were also added to the project by default:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image6.png)

<span data-ttu-id="6fd23-145">Quando si espande il /Content e le directory /script, è possibile trovare un file Site CSS utilizzato per applicare uno stile a tutto il codice HTML nel sito, nonché librerie JavaScript che è possono abilitare ASP.NET AJAX e jQuery supportano all'interno dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="6fd23-145">When we expand the /Content and /Scripts directories, we'll find a Site.css file that is used to style all HTML on the site, as well as JavaScript libraries that can enable ASP.NET AJAX and jQuery support within the application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image7.png)

<span data-ttu-id="6fd23-146">Quando si espande il progetto NerdDinner.Tests verrà individuato due classi che contengono gli unit test per le classi controller:</span><span class="sxs-lookup"><span data-stu-id="6fd23-146">When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image8.png)

<span data-ttu-id="6fd23-147">Questi file predefinito aggiunti da Visual Studio forniscono una struttura di base per un'applicazione funzionante - completa con una pagina home, sulla pagina, le pagine di accesso/disconnessione/registrazione account e una pagina di errore non gestito (tutto dopo aver e lavoro impostazione predefinita).</span><span class="sxs-lookup"><span data-stu-id="6fd23-147">These default files added by Visual Studio provide us with a basic structure for a working application - complete with home page, about page, account login/logout/registration pages, and an unhandled error page (all wired-up and working out of the box).</span></span>

### <a name="running-the-nerddinner-application"></a><span data-ttu-id="6fd23-148">Esecuzione dell'applicazione di NerdDinner</span><span class="sxs-lookup"><span data-stu-id="6fd23-148">Running the NerdDinner Application</span></span>

<span data-ttu-id="6fd23-149">È possibile eseguire il progetto scegliendo tra le **Debug -&gt;Avvia debug** oppure **Debug -&gt;Avvia senza eseguire debug** voci di menu:</span><span class="sxs-lookup"><span data-stu-id="6fd23-149">We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image9.png)

<span data-ttu-id="6fd23-150">Questo verrà avviare il server Web ASP.NET predefinito-fornito con Visual Studio ed eseguire l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="6fd23-150">This will launch the built-in ASP.NET Web-server that comes with Visual Studio, and run our application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image10.png)

<span data-ttu-id="6fd23-151">Di seguito è la home page per il nuovo progetto (URL: "/") al momento dell'esecuzione:</span><span class="sxs-lookup"><span data-stu-id="6fd23-151">Below is the home page for our new project (URL: "/") when it runs:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image11.png)

<span data-ttu-id="6fd23-152">Facendo clic sulla scheda "About" consente di visualizzare sulla pagina (URL: "/ Home/About"):</span><span class="sxs-lookup"><span data-stu-id="6fd23-152">Clicking the "About" tab displays an about page (URL: "/Home/About"):</span></span>

![](create-a-new-aspnet-mvc-project/_static/image12.png)

<span data-ttu-id="6fd23-153">Facendo clic sul collegamento "Accedi" in alto a destra, viene visualizzata una pagina di accesso (URL: "/ Account/LogOn")</span><span class="sxs-lookup"><span data-stu-id="6fd23-153">Clicking the "Log On" link on the top-right takes us to a Login page (URL: "/Account/LogOn")</span></span>

![](create-a-new-aspnet-mvc-project/_static/image13.png)

<span data-ttu-id="6fd23-154">Se si ha un account di accesso è possibile fare clic sul collegamento register (URL: "/ Account/Register") per crearne uno:</span><span class="sxs-lookup"><span data-stu-id="6fd23-154">If we don't have a login account we can click the register link (URL: "/Account/Register") to create one:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image14.png)

<span data-ttu-id="6fd23-155">Il codice per implementare la precedente home page, e la disconnessione o registrare la funzionalità è stata aggiunta per impostazione predefinita, quando abbiamo creato il nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="6fd23-155">The code to implement the above home, about, and logout/ register functionality was added by default when we created our new project.</span></span> <span data-ttu-id="6fd23-156">Si userà lo come punto di partenza dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="6fd23-156">We'll use it as the starting point of our application.</span></span>

### <a name="testing-the-nerddinner-application"></a><span data-ttu-id="6fd23-157">Test dell'applicazione di NerdDinner</span><span class="sxs-lookup"><span data-stu-id="6fd23-157">Testing the NerdDinner Application</span></span>

<span data-ttu-id="6fd23-158">Se si sta usando la Professional Edition o versione successiva di Visual Studio 2008, è possibile usare l'unità predefinito test supporto IDE di Visual Studio per il progetto di test:</span><span class="sxs-lookup"><span data-stu-id="6fd23-158">If we are using the Professional Edition or higher version of Visual Studio 2008, we can use the built-in unit testing IDE support within Visual Studio to test the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image15.png)

<span data-ttu-id="6fd23-159">Scegliere una delle opzioni precedenti verranno aprire il riquadro "Risultati dei Test" all'interno dell'IDE e fornire lo stato di esito positivo o negativo nel 27 unit test inclusi nel nostro nuovo progetto che coprono le funzionalità predefinite:</span><span class="sxs-lookup"><span data-stu-id="6fd23-159">Choosing one of the above options will open the "Test Results" pane within the IDE and provide us with pass/fail status on the 27 unit tests included in our new project that cover the built-in functionality:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image16.png)

<span data-ttu-id="6fd23-160">Più avanti in questa esercitazione verranno fornite altre informazioni di test automatizzati e aggiungere ulteriori unit test che coprono le funzionalità dell'applicazione, che abbiamo implementato.</span><span class="sxs-lookup"><span data-stu-id="6fd23-160">Later in this tutorial we'll talk more about automated testing and add additional unit tests that cover the application functionality we implement.</span></span>

### <a name="next-step"></a><span data-ttu-id="6fd23-161">Passo successivo</span><span class="sxs-lookup"><span data-stu-id="6fd23-161">Next Step</span></span>

<span data-ttu-id="6fd23-162">A questo punto, abbiamo una struttura di base dell'applicazione posto.</span><span class="sxs-lookup"><span data-stu-id="6fd23-162">We've now got a basic application structure in place.</span></span> <span data-ttu-id="6fd23-163">È possibile ora [creare un database per archiviare i dati applicazione](create-a-database.md).</span><span class="sxs-lookup"><span data-stu-id="6fd23-163">Let's now [create a database to store our application data](create-a-database.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6fd23-164">[Precedente](introducing-the-nerddinner-tutorial.md)
> [Successivo](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="6fd23-164">[Previous](introducing-the-nerddinner-tutorial.md)
[Next](create-a-database.md)</span></span>
