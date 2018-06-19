---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Esercitazione: Introduzione a SignalR 2 e MVC 5 | Documenti Microsoft'
author: pfletcher
description: In questa esercitazione viene illustrato come utilizzare ASP.NET SignalR 2 per creare un'applicazione di chat in tempo reale. Si verrà aggiunto SignalR a un'applicazione MVC 5 e creare una vista di chat...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: ca0471114da7363c5df9d459308708e7ab4f8b84
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
ms.locfileid: "28033847"
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a><span data-ttu-id="21518-104">Esercitazione: Introduzione a SignalR 2 e MVC 5</span><span class="sxs-lookup"><span data-stu-id="21518-104">Tutorial: Getting Started with SignalR 2 and MVC 5</span></span>
====================
<span data-ttu-id="21518-105">da [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="21518-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[<span data-ttu-id="21518-106">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="21518-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> <span data-ttu-id="21518-107">In questa esercitazione viene illustrato come utilizzare ASP.NET SignalR 2 per creare un'applicazione di chat in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="21518-107">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="21518-108">Verrà aggiungere SignalR a un'applicazione MVC 5 e creare una vista di chat per inviare e visualizzare i messaggi.</span><span class="sxs-lookup"><span data-stu-id="21518-108">You will add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span> 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="21518-109">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="21518-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="21518-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="21518-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="21518-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="21518-111">.NET 4.5</span></span>
> - <span data-ttu-id="21518-112">MVC 5</span><span class="sxs-lookup"><span data-stu-id="21518-112">MVC 5</span></span>
> - <span data-ttu-id="21518-113">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="21518-113">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="21518-114">Utilizzo di Visual Studio 2012 con questa esercitazione</span><span class="sxs-lookup"><span data-stu-id="21518-114">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="21518-115">Per utilizzare Visual Studio 2012 con questa esercitazione, effettuare le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="21518-115">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="21518-116">Aggiornamento del [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) alla versione più recente.</span><span class="sxs-lookup"><span data-stu-id="21518-116">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="21518-117">Installare il [installazione guidata piattaforma Web](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="21518-117">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="21518-118">L'installazione guidata piattaforma Web, cercare e installare **ASP.NET e 2013.1 strumenti Web per Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="21518-118">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="21518-119">Verrà installato come modelli di Visual Studio per le classi di SignalR **Hub**.</span><span class="sxs-lookup"><span data-stu-id="21518-119">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="21518-120">Alcuni modelli (ad esempio **classe di avvio di OWIN**) non saranno disponibili per tali file, usare un file di classe.</span><span class="sxs-lookup"><span data-stu-id="21518-120">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="21518-121">Versioni dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="21518-121">Tutorial Versions</span></span>
> 
> <span data-ttu-id="21518-122">Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="21518-122">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="21518-123">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="21518-123">Questions and comments</span></span>
> 
> <span data-ttu-id="21518-124">Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="21518-124">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="21518-125">In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="21518-125">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="21518-126">Panoramica</span><span class="sxs-lookup"><span data-stu-id="21518-126">Overview</span></span>

<span data-ttu-id="21518-127">Questa esercitazione viene illustrato lo sviluppo di applicazioni web in tempo reale con ASP.NET SignalR 2 e MVC ASP.NET 5.</span><span class="sxs-lookup"><span data-stu-id="21518-127">This tutorial introduces you to real-time web application development with ASP.NET SignalR 2 and ASP.NET MVC 5.</span></span> <span data-ttu-id="21518-128">L'esercitazione Usa lo stesso codice applicazione chat di [esercitazione introduttiva SignalR](tutorial-getting-started-with-signalr.md), ma viene illustrato come aggiungerlo a un'applicazione MVC 5.</span><span class="sxs-lookup"><span data-stu-id="21518-128">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 5 application.</span></span>

<span data-ttu-id="21518-129">In questo argomento verranno illustrate le attività di sviluppo SignalR seguenti:</span><span class="sxs-lookup"><span data-stu-id="21518-129">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="21518-130">Aggiunta della libreria di SignalR a un'applicazione MVC 5.</span><span class="sxs-lookup"><span data-stu-id="21518-130">Adding the SignalR library to an MVC 5 application.</span></span>
- <span data-ttu-id="21518-131">Creazione di hub e le classi di avvio OWIN per effettuare il push del contenuto ai client.</span><span class="sxs-lookup"><span data-stu-id="21518-131">Creating hub and OWIN startup classes to push content to clients.</span></span>
- <span data-ttu-id="21518-132">Utilizzando la libreria jQuery SignalR in una pagina web per inviare messaggi e visualizzare gli aggiornamenti dall'hub.</span><span class="sxs-lookup"><span data-stu-id="21518-132">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="21518-133">La schermata seguente viene illustrata l'applicazione di chat completata in esecuzione in un browser.</span><span class="sxs-lookup"><span data-stu-id="21518-133">The following screen shot shows the completed chat application running in a browser.</span></span>

![Istanze di chat](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

<span data-ttu-id="21518-135">Nelle sezioni:</span><span class="sxs-lookup"><span data-stu-id="21518-135">Sections:</span></span>

- [<span data-ttu-id="21518-136">Impostare il progetto</span><span class="sxs-lookup"><span data-stu-id="21518-136">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="21518-137">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="21518-137">Run the Sample</span></span>](#run)
- [<span data-ttu-id="21518-138">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="21518-138">Examine the Code</span></span>](#code)
- [<span data-ttu-id="21518-139">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="21518-139">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="21518-140">Impostare il progetto</span><span class="sxs-lookup"><span data-stu-id="21518-140">Set up the Project</span></span>

<span data-ttu-id="21518-141">Prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="21518-141">Prerequisites:</span></span>

- <span data-ttu-id="21518-142">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="21518-142">Visual Studio 2013.</span></span> <span data-ttu-id="21518-143">Se non si dispone di Visual Studio, vedere [Scarica ASP.NET](https://www.asp.net/downloads) per ottenere il libero Visual Studio 2013 Express strumento di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="21518-143">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express Development Tool.</span></span>

<span data-ttu-id="21518-144">In questa sezione viene illustrato come creare un'applicazione ASP.NET MVC 5, aggiungere la libreria SignalR e creare l'applicazione di chat.</span><span class="sxs-lookup"><span data-stu-id="21518-144">This section shows how to create an ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="21518-145">In Visual Studio, creare un'applicazione c# ASP.NET destinato a .NET Framework 4.5, denominarla SignalRChat e fare clic su OK.</span><span class="sxs-lookup"><span data-stu-id="21518-145">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![Creare web](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. <span data-ttu-id="21518-147">Nel `New ASP.NET Project` finestra di dialogo e selezionare **MVC**, fare clic su **Modifica autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="21518-147">In the `New ASP.NET Project` dialog, and select **MVC**, and click **Change Authentication**.</span></span>

    ![Creare web](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. <span data-ttu-id="21518-149">Selezionare **Nessuna autenticazione** nel **Modifica autenticazione** finestra di dialogo e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="21518-149">Select **No Authentication** in the **Change Authentication** dialog, and click **OK**.</span></span>

    ![Non selezionare Nessuna autenticazione](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > <span data-ttu-id="21518-151">Se si seleziona un provider di autenticazione diversi per l'applicazione, un `Startup.cs` classe verrà creata automaticamente; non è necessario crearne di proprie `Startup.cs` classe nel passaggio 10 seguente.</span><span class="sxs-lookup"><span data-stu-id="21518-151">If you select a different authentication provider for your application, a `Startup.cs` class will be created for you; you will not need to create your own `Startup.cs` class in step 10 below.</span></span>
4. <span data-ttu-id="21518-152">Fare clic su **OK** nel **nuovo progetto ASP.NET** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="21518-152">Click **OK** in the **New ASP.NET Project** dialog.</span></span>
5. <span data-ttu-id="21518-153">Aprire il **strumenti | Gestione pacchetti libreria | Console di gestione pacchetti** ed eseguire il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="21518-153">Open the **Tools | Library Package Manager | Package Manager Console** and run the following command.</span></span> <span data-ttu-id="21518-154">Questo passaggio viene aggiunto al progetto un set di file di script e i riferimenti ad assembly che abilita la funzionalità di SignalR.</span><span class="sxs-lookup"><span data-stu-id="21518-154">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

    `install-package Microsoft.AspNet.SignalR`
6. <span data-ttu-id="21518-155">In **Esplora**, espandere la cartella degli script.</span><span class="sxs-lookup"><span data-stu-id="21518-155">In **Solution Explorer**, expand the Scripts folder.</span></span> <span data-ttu-id="21518-156">Si noti che le librerie di script per SignalR siano stati aggiunti al progetto.</span><span class="sxs-lookup"><span data-stu-id="21518-156">Note that script libraries for SignalR have been added to the project.</span></span>

    ![Cartella degli script](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. <span data-ttu-id="21518-158">In **Esplora**, fare clic sul progetto, selezionare **Aggiungi | Nuova cartella**, e aggiungere una nuova cartella denominata **hub**.</span><span class="sxs-lookup"><span data-stu-id="21518-158">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
8. <span data-ttu-id="21518-159">Fare doppio clic su di **hub** cartella, fare clic su **Aggiungi | Nuovo elemento**, selezionare il **Visual c# | Web | SignalR** nodo il **installato** riquadro, selezionare **classe Hub SignalR (v2)** dal riquadro centrale e creare un nuovo hub denominato **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="21518-159">Right-click the **Hubs** folder, click **Add | New Item**, select the **Visual C# | Web | SignalR** node in the **Installed** pane, select **SignalR Hub Class (v2)** from the center pane, and create a new hub named **ChatHub.cs**.</span></span> <span data-ttu-id="21518-160">Utilizzare questa classe come un hub di SignalR server che invia messaggi a tutti i client.</span><span class="sxs-lookup"><span data-stu-id="21518-160">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

    ![Crea nuovo hub](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. <span data-ttu-id="21518-162">Sostituire il codice di **ChatHub** classe con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="21518-162">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. <span data-ttu-id="21518-163">Creare una nuova classe denominata Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="21518-163">Create a new class called Startup.cs.</span></span> <span data-ttu-id="21518-164">Modificare il contenuto del file per le operazioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="21518-164">Change the contents of the file to the following.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. <span data-ttu-id="21518-165">Modificare il `HomeController` classe trovata nel **Controllers/HomeController.cs** e aggiungere il metodo seguente alla classe.</span><span class="sxs-lookup"><span data-stu-id="21518-165">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="21518-166">Questo metodo restituisce il **Chat** visualizzazione che verrà creato in un passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="21518-166">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. <span data-ttu-id="21518-167">Fare doppio clic su di **Views/Home** cartella e selezionare **Aggiungi.... | Visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="21518-167">Right-click the **Views/Home** folder, and select **Add... | View**.</span></span>
13. <span data-ttu-id="21518-168">Nel **Aggiungi visualizzazione** finestra di dialogo, nome, la nuova vista **Chat**.</span><span class="sxs-lookup"><span data-stu-id="21518-168">In the **Add View** dialog, name the new view **Chat**.</span></span>

    ![Aggiungere una vista](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. <span data-ttu-id="21518-170">Sostituire il contenuto di **Chat.cshtml** con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="21518-170">Replace the contents of **Chat.cshtml** with the following code.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="21518-171">Quando si aggiungono SignalR e altre librerie di script al progetto di Visual Studio, la gestione dei pacchetti potrebbe installare una versione del file di script SignalR più recenti rispetto a quella illustrata in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="21518-171">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install a version of the SignalR script file that is more recent than the version shown in this topic.</span></span> <span data-ttu-id="21518-172">Assicurarsi che il riferimento allo script nel codice corrisponda alla versione della libreria di script installata nel progetto.</span><span class="sxs-lookup"><span data-stu-id="21518-172">Make sure that the script reference in your code matches the version of the script library installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. <span data-ttu-id="21518-173">**Salvare tutti** per il progetto.</span><span class="sxs-lookup"><span data-stu-id="21518-173">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="21518-174">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="21518-174">Run the Sample</span></span>

1. <span data-ttu-id="21518-175">Premere F5 per eseguire il progetto in modalità di debug.</span><span class="sxs-lookup"><span data-stu-id="21518-175">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="21518-176">Nella riga dell'indirizzo del browser, aggiungere **/home/chat** all'URL della pagina predefinita per il progetto.</span><span class="sxs-lookup"><span data-stu-id="21518-176">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="21518-177">Caricamento della pagina di Chat in un'istanza del browser e richiede un nome utente.</span><span class="sxs-lookup"><span data-stu-id="21518-177">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![Immettere il nome utente](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. <span data-ttu-id="21518-179">Immettere un nome utente.</span><span class="sxs-lookup"><span data-stu-id="21518-179">Enter a user name.</span></span>
4. <span data-ttu-id="21518-180">Copiare l'URL dalla riga dell'indirizzo del browser e utilizzarla per aprire le due altre istanze del browser.</span><span class="sxs-lookup"><span data-stu-id="21518-180">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="21518-181">In ogni istanza del browser, immettere un nome utente univoco.</span><span class="sxs-lookup"><span data-stu-id="21518-181">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="21518-182">In ogni istanza del browser, aggiungere un commento e fare clic su **inviare**.</span><span class="sxs-lookup"><span data-stu-id="21518-182">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="21518-183">I commenti deve essere visualizzato in tutte le istanze del browser.</span><span class="sxs-lookup"><span data-stu-id="21518-183">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="21518-184">Questa applicazione di una chat semplice non gestisce il contesto della discussione sul server.</span><span class="sxs-lookup"><span data-stu-id="21518-184">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="21518-185">L'hub trasmette i commenti per tutti gli utenti correnti.</span><span class="sxs-lookup"><span data-stu-id="21518-185">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="21518-186">Partecipare alla chat in un secondo momento gli utenti visualizzeranno messaggi aggiunti dal momento in cui che si connettono.</span><span class="sxs-lookup"><span data-stu-id="21518-186">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="21518-187">La schermata seguente viene illustrata l'applicazione di chat in esecuzione in un browser.</span><span class="sxs-lookup"><span data-stu-id="21518-187">The following screen shot shows the chat application running in a browser.</span></span>

    ![Browser chat](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. <span data-ttu-id="21518-189">In **Esplora**, controllare il **documenti Script** nodo per l'applicazione in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="21518-189">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="21518-190">Questo nodo viene visualizzato in modalità di debug, se si utilizza Internet Explorer come browser.</span><span class="sxs-lookup"><span data-stu-id="21518-190">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="21518-191">È presente un file script denominato **hub** che la libreria SignalR genera in modo dinamico in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="21518-191">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="21518-192">Questo file consente di gestire la comunicazione tra script jQuery e codice lato server.</span><span class="sxs-lookup"><span data-stu-id="21518-192">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="21518-193">Se si utilizza un browser diverso da Internet Explorer, è inoltre possibile accedere dinamica **hub** file passando ad esso direttamente, ad esempio http://mywebsite/signalr/hubs.</span><span class="sxs-lookup"><span data-stu-id="21518-193">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="21518-194">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="21518-194">Examine the Code</span></span>

<span data-ttu-id="21518-195">L'applicazione di chat SignalR illustra due attività di sviluppo SignalR base: creazione di un hub come oggetto di coordinamento principale sul server e utilizza la libreria di jQuery SignalR per inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="21518-195">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="21518-196">Hub di SignalR</span><span class="sxs-lookup"><span data-stu-id="21518-196">SignalR Hubs</span></span>

<span data-ttu-id="21518-197">Nell'esempio di codice il **ChatHub** deriva dalla classe di **Microsoft.AspNet.SignalR.Hub** classe.</span><span class="sxs-lookup"><span data-stu-id="21518-197">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="21518-198">Derivazione da di **Hub** classe è utile per compilare un'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="21518-198">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="21518-199">È possibile creare metodi pubblici nella classe hub e quindi accedere a tali metodi mediante chiamata dagli script in una pagina web.</span><span class="sxs-lookup"><span data-stu-id="21518-199">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="21518-200">Nel codice chat, i client chiamano il **ChatHub.Send** metodo per inviare un nuovo messaggio.</span><span class="sxs-lookup"><span data-stu-id="21518-200">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="21518-201">L'hub, a sua volta invia il messaggio a tutti i client chiamando **Clients.All.addNewMessageToPage**.</span><span class="sxs-lookup"><span data-stu-id="21518-201">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="21518-202">Il **inviare** metodo illustra i diversi concetti di hub:</span><span class="sxs-lookup"><span data-stu-id="21518-202">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="21518-203">Dichiarare i metodi pubblici in un hub in modo che i client possono essere richiamati.</span><span class="sxs-lookup"><span data-stu-id="21518-203">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="21518-204">Utilizzare il **Microsoft.AspNet.SignalR.Hub.Clients** proprietà per accedere a tutti i client connessi a questo hub.</span><span class="sxs-lookup"><span data-stu-id="21518-204">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="21518-205">Chiamare una funzione nel client (ad esempio il `addNewMessageToPage` (funzione)) per aggiornare i client.</span><span class="sxs-lookup"><span data-stu-id="21518-205">Call a function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="21518-206">SignalR e jQuery</span><span class="sxs-lookup"><span data-stu-id="21518-206">SignalR and jQuery</span></span>

<span data-ttu-id="21518-207">Il **Chat.cshtml** Visualizza file nell'esempio di codice viene illustrato come utilizzare la libreria jQuery SignalR per comunicare con un hub di SignalR.</span><span class="sxs-lookup"><span data-stu-id="21518-207">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="21518-208">Attività essenziali nel codice crea un riferimento per il proxy generato automaticamente per l'hub, dichiarazione di una funzione che il server è possibile chiamare per contenuto push ai client e all'avvio di una connessione per inviare messaggi all'hub.</span><span class="sxs-lookup"><span data-stu-id="21518-208">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="21518-209">Il codice seguente dichiara un riferimento a un proxy dell'hub.</span><span class="sxs-lookup"><span data-stu-id="21518-209">The following code declares a reference to a hub proxy.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="21518-210">In JavaScript, il riferimento alla classe del server e i relativi membri è in maiuscole-minuscole camel.</span><span class="sxs-lookup"><span data-stu-id="21518-210">In JavaScript the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="21518-211">L'esempio di codice fa riferimento a c# **ChatHub** classe in JavaScript come **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="21518-211">The code sample references the C# **ChatHub** class in JavaScript as **chatHub**.</span></span> <span data-ttu-id="21518-212">Se si desidera fare riferimento il `ChatHub` classe in jQuery con Pascal convenzionale maiuscole e minuscole come si farebbe in c#, modificare il file di classe ChatHub.cs.</span><span class="sxs-lookup"><span data-stu-id="21518-212">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="21518-213">Aggiungere un `using` istruzione per fare riferimento il `Microsoft.AspNet.SignalR.Hubs` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="21518-213">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="21518-214">Aggiungere quindi il `HubName` attributo la `ChatHub` classe, ad esempio `[HubName("ChatHub")]`.</span><span class="sxs-lookup"><span data-stu-id="21518-214">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="21518-215">Infine, aggiornare il riferimento jQuery la `ChatHub` classe.</span><span class="sxs-lookup"><span data-stu-id="21518-215">Finally, update your jQuery reference to the `ChatHub` class.</span></span>


<span data-ttu-id="21518-216">Il codice seguente viene illustrato come creare una funzione di callback nello script.</span><span class="sxs-lookup"><span data-stu-id="21518-216">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="21518-217">Classe dell'hub sul server chiama questa funzione per inviare gli aggiornamenti del contenuto di ogni client.</span><span class="sxs-lookup"><span data-stu-id="21518-217">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="21518-218">La chiamata a facoltativo di `htmlEncode` funzione Mostra un modo per HTML codifica il contenuto del messaggio prima di visualizzarlo nella pagina, come un modo per impedire attacchi script injection.</span><span class="sxs-lookup"><span data-stu-id="21518-218">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="21518-219">Il codice seguente viene illustrato come aprire una connessione con l'hub.</span><span class="sxs-lookup"><span data-stu-id="21518-219">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="21518-220">Il codice avvia la connessione e passa a una funzione per gestire l'evento click nel **inviare** nella pagina Chat.</span><span class="sxs-lookup"><span data-stu-id="21518-220">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="21518-221">Questo approccio assicura che la connessione viene stabilita prima dell'esecuzione il gestore dell'evento.</span><span class="sxs-lookup"><span data-stu-id="21518-221">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="21518-222">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="21518-222">Next Steps</span></span>

<span data-ttu-id="21518-223">Si è appreso che SignalR è un framework per la creazione di applicazioni web in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="21518-223">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="21518-224">È stato inoltre diverse attività di sviluppo SignalR: come aggiungere SignalR a un'applicazione ASP.NET, come creare una classe di hub e come inviare e ricevere messaggi dall'hub.</span><span class="sxs-lookup"><span data-stu-id="21518-224">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="21518-225">Per una procedura dettagliata su come distribuire l'applicazione di esempio SignalR in Azure, vedere [utilizzando SignalR con le app Web in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span><span class="sxs-lookup"><span data-stu-id="21518-225">For a walkthrough on how to deploy the sample SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="21518-226">Per informazioni dettagliate su come distribuire un progetto web di Visual Studio a un sito Web di Windows Azure, vedere [creare un'app web ASP.NET in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="21518-226">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<span data-ttu-id="21518-227">Per informazioni su concetti più avanzati di sviluppi SignalR, visitare i siti seguenti per il codice sorgente SignalR e risorse:</span><span class="sxs-lookup"><span data-stu-id="21518-227">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="21518-228">Progetto SignalR</span><span class="sxs-lookup"><span data-stu-id="21518-228">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="21518-229">SignalR Github ed esempi</span><span class="sxs-lookup"><span data-stu-id="21518-229">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="21518-230">Wiki di SignalR</span><span class="sxs-lookup"><span data-stu-id="21518-230">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
