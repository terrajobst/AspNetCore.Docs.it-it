---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Esercitazione: Introduzione a SignalR 2 | Documenti Microsoft'
author: pfletcher
description: In questa esercitazione viene illustrato come usare SignalR per creare un'applicazione di chat in tempo reale. Verrà aggiunta SignalR a un'applicazione web ASP.NET vuota e verrà creato un pa HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 8be851f5a2b1cca39f5f8f284ff1c002c486d7e8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-2"></a><span data-ttu-id="51eae-104">Esercitazione: Introduzione a SignalR 2</span><span class="sxs-lookup"><span data-stu-id="51eae-104">Tutorial: Getting Started with SignalR 2</span></span>
====================
<span data-ttu-id="51eae-105">da [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="51eae-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="51eae-106">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="51eae-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> <span data-ttu-id="51eae-107">In questa esercitazione viene illustrato come usare SignalR per creare un'applicazione di chat in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="51eae-107">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="51eae-108">Verrà aggiunta SignalR a un'applicazione web ASP.NET vuota e creare una pagina HTML per l'invio e visualizzare i messaggi.</span><span class="sxs-lookup"><span data-stu-id="51eae-108">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span> 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="51eae-109">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="51eae-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="51eae-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="51eae-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="51eae-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="51eae-111">.NET 4.5</span></span>
> - <span data-ttu-id="51eae-112">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="51eae-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="51eae-113">Utilizzo di Visual Studio 2012 con questa esercitazione</span><span class="sxs-lookup"><span data-stu-id="51eae-113">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="51eae-114">Per utilizzare Visual Studio 2012 con questa esercitazione, effettuare le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="51eae-114">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="51eae-115">Aggiornamento del [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) alla versione più recente.</span><span class="sxs-lookup"><span data-stu-id="51eae-115">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="51eae-116">Installare il [installazione guidata piattaforma Web](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="51eae-116">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="51eae-117">L'installazione guidata piattaforma Web, cercare e installare **ASP.NET e 2013.1 strumenti Web per Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="51eae-117">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="51eae-118">Verrà installato come modelli di Visual Studio per le classi di SignalR **Hub**.</span><span class="sxs-lookup"><span data-stu-id="51eae-118">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="51eae-119">Alcuni modelli (ad esempio **classe di avvio di OWIN**) non saranno disponibili per tali file, usare un file di classe.</span><span class="sxs-lookup"><span data-stu-id="51eae-119">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="51eae-120">Versioni dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="51eae-120">Tutorial versions</span></span>
> 
> <span data-ttu-id="51eae-121">Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="51eae-121">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="51eae-122">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="51eae-122">Questions and comments</span></span>
> 
> <span data-ttu-id="51eae-123">Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="51eae-123">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="51eae-124">In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="51eae-124">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="51eae-125">Panoramica</span><span class="sxs-lookup"><span data-stu-id="51eae-125">Overview</span></span>

<span data-ttu-id="51eae-126">Questa esercitazione viene illustrato lo sviluppo di SignalR viene mostrato come compilare un'applicazione di una chat semplice basato su browser.</span><span class="sxs-lookup"><span data-stu-id="51eae-126">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="51eae-127">Si verrà aggiungere la libreria SignalR a un'applicazione web ASP.NET vuota, creare una classe di hub per l'invio di messaggi al client e creare una pagina HTML che consente agli utenti di inviare e ricevere messaggi di chat.</span><span class="sxs-lookup"><span data-stu-id="51eae-127">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="51eae-128">Per un'esercitazione simile che illustra come creare un'applicazione di chat in MVC 5 con una visualizzazione MVC, vedere [introduzione SignalR 2 e MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="51eae-128">For a similar tutorial that shows how to create a chat application in MVC 5 using an MVC view, see [Getting Started with SignalR 2 and MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

> [!NOTE]
> <span data-ttu-id="51eae-129">Questa esercitazione viene illustrato come creare applicazioni SignalR in versione 2.</span><span class="sxs-lookup"><span data-stu-id="51eae-129">This tutorial demonstrates how to create SignalR applications in version 2.</span></span> <span data-ttu-id="51eae-130">Per informazioni dettagliate sulle modifiche tra SignalR 1. x e 2, vedere [SignalR aggiornamento 1. x progetti](../releases/upgrading-signalr-1x-projects-to-20.md) e [note sulla versione di Visual Studio 2013](../../../visual-studio/overview/2013/release-notes.md#TOC13).</span><span class="sxs-lookup"><span data-stu-id="51eae-130">For details on changes between SignalR 1.x and 2, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md) and [Visual Studio 2013 Release Notes](../../../visual-studio/overview/2013/release-notes.md#TOC13).</span></span>

<span data-ttu-id="51eae-131">SignalR è una libreria .NET open source per la compilazione di applicazioni web che richiedono l'intervento dell'utente in tempo reale o gli aggiornamenti dei dati in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="51eae-131">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="51eae-132">Sono esempi di applicazioni social networking, giochi multiutente, meteo di collaborazione e notizie, business o finanziari aggiornamento delle applicazioni.</span><span class="sxs-lookup"><span data-stu-id="51eae-132">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="51eae-133">Spesso si tratta di applicazioni in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="51eae-133">These are often called real-time applications.</span></span>

<span data-ttu-id="51eae-134">SignalR semplifica il processo di compilazione di applicazioni in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="51eae-134">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="51eae-135">Include una libreria del server ASP.NET e una libreria client JavaScript per renderne più semplice gestire le connessioni client-server e inviare gli aggiornamenti del contenuto ai client.</span><span class="sxs-lookup"><span data-stu-id="51eae-135">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="51eae-136">È possibile aggiungere la libreria SignalR a un'applicazione ASP.NET esistente per ottenere le funzionalità in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="51eae-136">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="51eae-137">Nell'esercitazione vengono illustrate le seguenti attività di sviluppo SignalR:</span><span class="sxs-lookup"><span data-stu-id="51eae-137">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="51eae-138">Aggiunta della libreria di SignalR a un'applicazione web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="51eae-138">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="51eae-139">Creazione di una classe di hub per eseguire il push del contenuto ai client.</span><span class="sxs-lookup"><span data-stu-id="51eae-139">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="51eae-140">Creazione di una classe di avvio OWIN per configurare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="51eae-140">Creating an OWIN startup class to configure the application.</span></span>
- <span data-ttu-id="51eae-141">Utilizzando la libreria jQuery SignalR in una pagina web per inviare messaggi e visualizzare gli aggiornamenti dall'hub.</span><span class="sxs-lookup"><span data-stu-id="51eae-141">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="51eae-142">La schermata seguente viene illustrata l'applicazione di chat in esecuzione in un browser.</span><span class="sxs-lookup"><span data-stu-id="51eae-142">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="51eae-143">Ogni nuovo utente è possibile inviare commenti e vedere i commenti aggiunti dopo che l'utente viene aggiunto alla chat.</span><span class="sxs-lookup"><span data-stu-id="51eae-143">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Istanze di chat](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="51eae-145">Nelle sezioni:</span><span class="sxs-lookup"><span data-stu-id="51eae-145">Sections:</span></span>

- [<span data-ttu-id="51eae-146">Impostare il progetto</span><span class="sxs-lookup"><span data-stu-id="51eae-146">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="51eae-147">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="51eae-147">Run the Sample</span></span>](#run)
- [<span data-ttu-id="51eae-148">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="51eae-148">Examine the Code</span></span>](#code)
- [<span data-ttu-id="51eae-149">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="51eae-149">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="51eae-150">Impostare il progetto</span><span class="sxs-lookup"><span data-stu-id="51eae-150">Set up the Project</span></span>

<span data-ttu-id="51eae-151">In questa sezione viene illustrato come utilizzare Visual Studio 2013 e SignalR versione 2 per creare applicazioni web ASP.NET vuota aggiungere SignalR e creare l'applicazione di chat.</span><span class="sxs-lookup"><span data-stu-id="51eae-151">This section shows how to use Visual Studio 2013 and SignalR version 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="51eae-152">Prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="51eae-152">Prerequisites:</span></span>

- <span data-ttu-id="51eae-153">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="51eae-153">Visual Studio 2013.</span></span> <span data-ttu-id="51eae-154">Se non si dispone di Visual Studio, vedere [Scarica ASP.NET](https://www.asp.net/downloads) per ottenere il libero Visual Studio 2013 Express strumento di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="51eae-154">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express Development Tool.</span></span>

<span data-ttu-id="51eae-155">La procedura seguente utilizza Visual Studio 2013 per creare un'applicazione Web vuota ASP.NET e aggiungere la libreria SignalR:</span><span class="sxs-lookup"><span data-stu-id="51eae-155">The following steps use Visual Studio 2013 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="51eae-156">In Visual Studio, creare un'applicazione Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="51eae-156">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Creare web](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="51eae-158">Nel **nuovo progetto ASP.NET** finestra, lasciare **vuoto** selezionata e fare clic su **Crea progetto**.</span><span class="sxs-lookup"><span data-stu-id="51eae-158">In the **New ASP.NET Project** window, leave **Empty** selected and click **Create Project**.</span></span>

    ![Creare web vuoto](tutorial-getting-started-with-signalr/_static/image3.png)
3. <span data-ttu-id="51eae-160">In **Esplora**, fare clic sul progetto, selezionare **Aggiungi | Classe Hub SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="51eae-160">In **Solution Explorer**, right-click the project, select **Add | SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="51eae-161">Nome della classe **ChatHub.cs** e aggiungerlo al progetto.</span><span class="sxs-lookup"><span data-stu-id="51eae-161">Name the class **ChatHub.cs** and add it to the project.</span></span> <span data-ttu-id="51eae-162">Questo passaggio viene creata la **ChatHub** classe e lo aggiunge al progetto un set di file di script e i riferimenti ad assembly che supportano SignalR.</span><span class="sxs-lookup"><span data-stu-id="51eae-162">This step creates the **ChatHub** class and adds to the project a set of script files and assembly references that support SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="51eae-163">È anche possibile aggiungere SignalR a un progetto, aprire il **strumenti | Gestione pacchetti libreria | Console di gestione pacchetti** ed eseguendo un comando:</span><span class="sxs-lookup"><span data-stu-id="51eae-163">You can also add SignalR to a project by opening the **Tools | Library Package Manager | Package Manager Console** and running a command:</span></span>

    `install-package Microsoft.AspNet.SignalR`

    <span data-ttu-id="51eae-164">Se si utilizza la console per aggiungere SignalR, creare la classe di hub SignalR come passaggio separato dopo l'aggiunta di SignalR.</span><span class="sxs-lookup"><span data-stu-id="51eae-164">If you use the console to add SignalR, create the SignalR hub class as a separate step after you add SignalR.</span></span>

    > [!NOTE]
    > <span data-ttu-id="51eae-165">Se si utilizza Visual Studio 2012, la **classe Hub SignalR (v2)** modello non sarà disponibile.</span><span class="sxs-lookup"><span data-stu-id="51eae-165">If you are using Visual Studio 2012, the **SignalR Hub Class (v2)** template will not be available.</span></span> <span data-ttu-id="51eae-166">È possibile aggiungere un normale **classe** chiamato `ChatHub` invece.</span><span class="sxs-lookup"><span data-stu-id="51eae-166">You can add a plain **Class** called `ChatHub` instead.</span></span>
4. <span data-ttu-id="51eae-167">In **Esplora**, espandere il nodo di script.</span><span class="sxs-lookup"><span data-stu-id="51eae-167">In **Solution Explorer**, expand the Scripts node.</span></span> <span data-ttu-id="51eae-168">Librerie di script per jQuery e SignalR sono visibili nel progetto.</span><span class="sxs-lookup"><span data-stu-id="51eae-168">Script libraries for jQuery and SignalR are visible in the project.</span></span>
5. <span data-ttu-id="51eae-169">Sostituire il codice nella nuova **ChatHub** classe con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="51eae-169">Replace the code in the new **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="51eae-170">In **Esplora**, fare clic sul progetto, quindi fare clic su **Aggiungi | Classe di avvio OWIN**.</span><span class="sxs-lookup"><span data-stu-id="51eae-170">In **Solution Explorer**, right-click the project, then click **Add | OWIN Startup Class**.</span></span> <span data-ttu-id="51eae-171">Denominare la nuova classe `Startup` e fare clic su OK.</span><span class="sxs-lookup"><span data-stu-id="51eae-171">Name the new class `Startup` and click OK.</span></span>

    > [!NOTE]
    > <span data-ttu-id="51eae-172">Se si utilizza Visual Studio 2012, la **classe di avvio di OWIN** modello non sarà disponibile.</span><span class="sxs-lookup"><span data-stu-id="51eae-172">If you are using Visual Studio 2012, the **OWIN Startup Class** template will not be available.</span></span> <span data-ttu-id="51eae-173">È possibile aggiungere un normale **classe** chiamato `Startup` invece.</span><span class="sxs-lookup"><span data-stu-id="51eae-173">You can add a plain **Class** called `Startup` instead.</span></span>
7. <span data-ttu-id="51eae-174">Modificare il contenuto della nuova classe di avvio per le operazioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="51eae-174">Change the contents of the new Startup class to the following.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="51eae-175">In **Esplora**, fare clic sul progetto, quindi fare clic su **Aggiungi | Pagina HTML**.</span><span class="sxs-lookup"><span data-stu-id="51eae-175">In **Solution Explorer**, right-click the project, then click **Add | HTML Page**.</span></span> <span data-ttu-id="51eae-176">Denominare la nuova pagina `index.html`.</span><span class="sxs-lookup"><span data-stu-id="51eae-176">Name the new page `index.html`.</span></span>
    >[!NOTE]
    ><span data-ttu-id="51eae-177">Potrebbe essere necessario modificare i numeri di versione per i riferimenti alle librerie JQuery e SignalR</span><span class="sxs-lookup"><span data-stu-id="51eae-177">You might need to change the version numbers for the references to JQuery and SignalR libraries</span></span>
9. <span data-ttu-id="51eae-178">In **Esplora**, fare clic sulla pagina HTML appena creato e fare clic su **imposta come pagina iniziale**.</span><span class="sxs-lookup"><span data-stu-id="51eae-178">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
10. <span data-ttu-id="51eae-179">Sostituire il codice predefinito nella pagina HTML con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="51eae-179">Replace the default code in the HTML page with the following code.</span></span>

    > [!NOTE]
    > <span data-ttu-id="51eae-180">Una versione successiva degli script SignalR può essere installata mediante la gestione dei pacchetti.</span><span class="sxs-lookup"><span data-stu-id="51eae-180">A later version of the SignalR scripts may be installed by the package manager.</span></span> <span data-ttu-id="51eae-181">Verificare che i riferimenti a script riportato di seguito corrispondono alle versioni dei file di script nel progetto (sarà diversi se è stato aggiunto mediante NuGet anziché aggiungendo un hub di SignalR.)</span><span class="sxs-lookup"><span data-stu-id="51eae-181">Verify that the script references below correspond to the versions of the script files in the project (they will be different if you added SignalR using NuGet rather than adding a hub.)</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. <span data-ttu-id="51eae-182">**Salvare tutti** per il progetto.</span><span class="sxs-lookup"><span data-stu-id="51eae-182">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="51eae-183">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="51eae-183">Run the Sample</span></span>

1. <span data-ttu-id="51eae-184">Premere F5 per eseguire il progetto in modalità di debug.</span><span class="sxs-lookup"><span data-stu-id="51eae-184">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="51eae-185">La pagina HTML viene caricato in un'istanza del browser e richiede un nome utente.</span><span class="sxs-lookup"><span data-stu-id="51eae-185">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Immettere il nome utente](tutorial-getting-started-with-signalr/_static/image4.png)
2. <span data-ttu-id="51eae-187">Immettere un nome utente.</span><span class="sxs-lookup"><span data-stu-id="51eae-187">Enter a user name.</span></span>
3. <span data-ttu-id="51eae-188">Copiare l'URL dalla riga dell'indirizzo del browser e utilizzarla per aprire le due altre istanze del browser.</span><span class="sxs-lookup"><span data-stu-id="51eae-188">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="51eae-189">In ogni istanza del browser, immettere un nome utente univoco.</span><span class="sxs-lookup"><span data-stu-id="51eae-189">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="51eae-190">In ogni istanza del browser, aggiungere un commento e fare clic su **inviare**.</span><span class="sxs-lookup"><span data-stu-id="51eae-190">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="51eae-191">I commenti deve essere visualizzato in tutte le istanze del browser.</span><span class="sxs-lookup"><span data-stu-id="51eae-191">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="51eae-192">Questa applicazione di una chat semplice non gestisce il contesto della discussione sul server.</span><span class="sxs-lookup"><span data-stu-id="51eae-192">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="51eae-193">L'hub trasmette i commenti per tutti gli utenti correnti.</span><span class="sxs-lookup"><span data-stu-id="51eae-193">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="51eae-194">Partecipare alla chat in un secondo momento gli utenti visualizzeranno messaggi aggiunti dal momento in cui che si connettono.</span><span class="sxs-lookup"><span data-stu-id="51eae-194">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="51eae-195">La schermata seguente viene illustrata l'applicazione di chat in esecuzione in tre istanze del browser, ognuno dei quali vengono aggiornati quando un'istanza invia un messaggio:</span><span class="sxs-lookup"><span data-stu-id="51eae-195">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Browser chat](tutorial-getting-started-with-signalr/_static/image5.png)
5. <span data-ttu-id="51eae-197">In **Esplora**, controllare il **documenti Script** nodo per l'applicazione in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="51eae-197">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="51eae-198">È presente un file script denominato **hub** che la libreria SignalR genera in modo dinamico in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="51eae-198">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="51eae-199">Questo file consente di gestire la comunicazione tra script jQuery e codice lato server.</span><span class="sxs-lookup"><span data-stu-id="51eae-199">This file manages the communication between jQuery script and server-side code.</span></span>

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="51eae-200">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="51eae-200">Examine the Code</span></span>

<span data-ttu-id="51eae-201">L'applicazione di chat SignalR illustra due attività di sviluppo SignalR base: creazione di un hub come oggetto di coordinamento principale sul server e utilizza la libreria di jQuery SignalR per inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="51eae-201">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="51eae-202">Hub di SignalR</span><span class="sxs-lookup"><span data-stu-id="51eae-202">SignalR Hubs</span></span>

<span data-ttu-id="51eae-203">Nell'esempio di codice il **ChatHub** deriva dalla classe di **Microsoft.AspNet.SignalR.Hub** classe.</span><span class="sxs-lookup"><span data-stu-id="51eae-203">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="51eae-204">Derivazione da di **Hub** classe è utile per compilare un'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="51eae-204">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="51eae-205">È possibile creare metodi pubblici nella classe hub e quindi accedere a tali metodi mediante chiamata dagli script in una pagina web.</span><span class="sxs-lookup"><span data-stu-id="51eae-205">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="51eae-206">Nel codice chat, i client chiamano il **ChatHub.Send** metodo per inviare un nuovo messaggio.</span><span class="sxs-lookup"><span data-stu-id="51eae-206">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="51eae-207">L'hub, a sua volta invia il messaggio a tutti i client chiamando **Clients.All.broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="51eae-207">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="51eae-208">Il **inviare** metodo illustra i diversi concetti di hub:</span><span class="sxs-lookup"><span data-stu-id="51eae-208">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="51eae-209">Dichiarare i metodi pubblici in un hub in modo che i client possono essere richiamati.</span><span class="sxs-lookup"><span data-stu-id="51eae-209">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="51eae-210">Utilizzare il **Microsoft.AspNet.SignalR.Hub.Clients** proprietà dinamiche per accedere a tutti i client connessi a questo hub.</span><span class="sxs-lookup"><span data-stu-id="51eae-210">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="51eae-211">Chiamare una funzione nel client (ad esempio il `broadcastMessage` (funzione)) per aggiornare i client.</span><span class="sxs-lookup"><span data-stu-id="51eae-211">Call a function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="51eae-212">SignalR e jQuery</span><span class="sxs-lookup"><span data-stu-id="51eae-212">SignalR and jQuery</span></span>

<span data-ttu-id="51eae-213">La pagina HTML nell'esempio di codice viene illustrato come utilizzare la libreria jQuery SignalR per comunicare con un hub di SignalR.</span><span class="sxs-lookup"><span data-stu-id="51eae-213">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="51eae-214">Attività essenziali nel codice sta dichiarando un proxy per fare riferimento all'hub, dichiarazione di una funzione che il server è possibile chiamare per contenuto push ai client e all'avvio di una connessione per inviare messaggi all'hub.</span><span class="sxs-lookup"><span data-stu-id="51eae-214">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="51eae-215">Il codice seguente dichiara un riferimento a un proxy dell'hub.</span><span class="sxs-lookup"><span data-stu-id="51eae-215">The following code declares a reference to a hub proxy.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="51eae-216">In JavaScript, il riferimento alla classe del server e i relativi membri è in maiuscole-minuscole camel.</span><span class="sxs-lookup"><span data-stu-id="51eae-216">In JavaScript the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="51eae-217">L'esempio di codice fa riferimento a c# **ChatHub** classe in JavaScript come **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="51eae-217">The code sample references the C# **ChatHub** class in JavaScript as **chatHub**.</span></span>


<span data-ttu-id="51eae-218">Il codice seguente è come si crea una funzione di callback nello script.</span><span class="sxs-lookup"><span data-stu-id="51eae-218">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="51eae-219">Classe dell'hub sul server chiama questa funzione per inviare gli aggiornamenti del contenuto di ogni client.</span><span class="sxs-lookup"><span data-stu-id="51eae-219">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="51eae-220">Le due righe che HTML codificare il contenuto prima di visualizzarlo sono facoltative e mostrano un modo semplice per impedire attacchi script injection.</span><span class="sxs-lookup"><span data-stu-id="51eae-220">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="51eae-221">Il codice seguente viene illustrato come aprire una connessione con l'hub.</span><span class="sxs-lookup"><span data-stu-id="51eae-221">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="51eae-222">Il codice avvia la connessione e passa a una funzione per gestire l'evento click nel **inviare** nella pagina HTML.</span><span class="sxs-lookup"><span data-stu-id="51eae-222">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="51eae-223">Questo approccio assicura che la connessione viene stabilita prima dell'esecuzione il gestore dell'evento.</span><span class="sxs-lookup"><span data-stu-id="51eae-223">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="51eae-224">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="51eae-224">Next Steps</span></span>

<span data-ttu-id="51eae-225">Si è appreso che SignalR è un framework per la creazione di applicazioni web in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="51eae-225">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="51eae-226">È stato inoltre diverse attività di sviluppo SignalR: come aggiungere SignalR a un'applicazione ASP.NET, come creare una classe di hub e come inviare e ricevere messaggi dall'hub.</span><span class="sxs-lookup"><span data-stu-id="51eae-226">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="51eae-227">Per una procedura dettagliata su come distribuire l'applicazione di esempio SignalR in Azure, vedere [utilizzando SignalR con le app Web in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span><span class="sxs-lookup"><span data-stu-id="51eae-227">For a walkthrough on how to deploy the sample SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="51eae-228">Per informazioni dettagliate su come distribuire un progetto web di Visual Studio a un sito Web di Windows Azure, vedere [creare un'app web ASP.NET in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="51eae-228">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<span data-ttu-id="51eae-229">Per informazioni su concetti più avanzati di sviluppi SignalR, visitare i siti seguenti per il codice sorgente SignalR e risorse:</span><span class="sxs-lookup"><span data-stu-id="51eae-229">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="51eae-230">Progetto SignalR</span><span class="sxs-lookup"><span data-stu-id="51eae-230">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="51eae-231">SignalR Github ed esempi</span><span class="sxs-lookup"><span data-stu-id="51eae-231">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="51eae-232">Wiki di SignalR</span><span class="sxs-lookup"><span data-stu-id="51eae-232">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
