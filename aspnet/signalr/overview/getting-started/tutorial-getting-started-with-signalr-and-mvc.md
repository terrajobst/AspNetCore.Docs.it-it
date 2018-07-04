---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Esercitazione: Introduzione a SignalR 2 e MVC 5 | Microsoft Docs'
author: pfletcher
description: Questa esercitazione illustra come usare ASP.NET SignalR 2 per creare un'applicazione di chat in tempo reale. Si verrà aggiunto SignalR a un'applicazione MVC 5 e creare una vista di chat...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: 903319040c9ac938cea5dce2e6579d88e0d80bb5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384051"
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a><span data-ttu-id="6e413-104">Esercitazione: Introduzione a SignalR 2 e MVC 5</span><span class="sxs-lookup"><span data-stu-id="6e413-104">Tutorial: Getting Started with SignalR 2 and MVC 5</span></span>
====================
<span data-ttu-id="6e413-105">dal [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="6e413-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[<span data-ttu-id="6e413-106">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="6e413-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> <span data-ttu-id="6e413-107">Questa esercitazione illustra come usare ASP.NET SignalR 2 per creare un'applicazione di chat in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="6e413-107">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="6e413-108">Verrà aggiunta SignalR a un'applicazione MVC 5 e creare una vista di chat per inviare e visualizzare i messaggi.</span><span class="sxs-lookup"><span data-stu-id="6e413-108">You will add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span> 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6e413-109">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="6e413-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="6e413-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="6e413-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="6e413-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="6e413-111">.NET 4.5</span></span>
> - <span data-ttu-id="6e413-112">MVC 5</span><span class="sxs-lookup"><span data-stu-id="6e413-112">MVC 5</span></span>
> - <span data-ttu-id="6e413-113">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="6e413-113">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="6e413-114">Uso di Visual Studio 2012 con questa esercitazione</span><span class="sxs-lookup"><span data-stu-id="6e413-114">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="6e413-115">Per usare Visual Studio 2012 con questa esercitazione, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="6e413-115">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="6e413-116">Aggiornamento di [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) alla versione più recente.</span><span class="sxs-lookup"><span data-stu-id="6e413-116">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="6e413-117">Installare il [installazione guidata piattaforma Web](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="6e413-117">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="6e413-118">L'installazione guidata piattaforma Web, cercare e installare **ASP.NET e Web Tools 2013.1 per Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="6e413-118">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="6e413-119">Verrà installato, ad esempio modelli di Visual Studio per le classi di SignalR **Hub**.</span><span class="sxs-lookup"><span data-stu-id="6e413-119">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="6e413-120">Alcuni modelli (ad esempio **classe di avvio OWIN**) non è disponibile; per questo motivo, usare invece un file di classe.</span><span class="sxs-lookup"><span data-stu-id="6e413-120">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="6e413-121">Versioni dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="6e413-121">Tutorial Versions</span></span>
> 
> <span data-ttu-id="6e413-122">Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="6e413-122">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="6e413-123">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="6e413-123">Questions and comments</span></span>
> 
> <span data-ttu-id="6e413-124">Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="6e413-124">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="6e413-125">Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="6e413-125">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="6e413-126">Panoramica</span><span class="sxs-lookup"><span data-stu-id="6e413-126">Overview</span></span>

<span data-ttu-id="6e413-127">Questa esercitazione viene illustrato lo sviluppo di applicazioni web in tempo reale con ASP.NET SignalR 2 e ASP.NET MVC 5.</span><span class="sxs-lookup"><span data-stu-id="6e413-127">This tutorial introduces you to real-time web application development with ASP.NET SignalR 2 and ASP.NET MVC 5.</span></span> <span data-ttu-id="6e413-128">L'esercitazione Usa lo stesso codice dell'applicazione di chat come le [esercitazione di introduzione a SignalR](tutorial-getting-started-with-signalr.md), ma illustra come aggiungerlo a un'applicazione MVC 5.</span><span class="sxs-lookup"><span data-stu-id="6e413-128">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 5 application.</span></span>

<span data-ttu-id="6e413-129">In questo argomento verranno fornite le seguenti attività di sviluppo SignalR:</span><span class="sxs-lookup"><span data-stu-id="6e413-129">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="6e413-130">Aggiunta della libreria di SignalR a un'applicazione MVC 5.</span><span class="sxs-lookup"><span data-stu-id="6e413-130">Adding the SignalR library to an MVC 5 application.</span></span>
- <span data-ttu-id="6e413-131">Creazione dell'hub e le classi di avvio OWIN per eseguire il push del contenuto ai client.</span><span class="sxs-lookup"><span data-stu-id="6e413-131">Creating hub and OWIN startup classes to push content to clients.</span></span>
- <span data-ttu-id="6e413-132">Usa la libreria jQuery SignalR in una pagina web per inviare messaggi e visualizzare gli aggiornamenti dall'hub.</span><span class="sxs-lookup"><span data-stu-id="6e413-132">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="6e413-133">Lo screenshot seguente mostra l'applicazione di chat completo in esecuzione in un browser.</span><span class="sxs-lookup"><span data-stu-id="6e413-133">The following screen shot shows the completed chat application running in a browser.</span></span>

![Istanze di chat](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

<span data-ttu-id="6e413-135">Nelle sezioni:</span><span class="sxs-lookup"><span data-stu-id="6e413-135">Sections:</span></span>

- [<span data-ttu-id="6e413-136">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="6e413-136">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="6e413-137">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="6e413-137">Run the Sample</span></span>](#run)
- [<span data-ttu-id="6e413-138">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="6e413-138">Examine the Code</span></span>](#code)
- [<span data-ttu-id="6e413-139">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="6e413-139">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="6e413-140">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="6e413-140">Set up the Project</span></span>

<span data-ttu-id="6e413-141">Prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="6e413-141">Prerequisites:</span></span>

- <span data-ttu-id="6e413-142">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="6e413-142">Visual Studio 2013.</span></span> <span data-ttu-id="6e413-143">Se non hai Visual Studio, vedere [download ASP.NET](https://www.asp.net/downloads) per ottenere il Visual Studio 2013 Express strumento di sviluppo gratuito.</span><span class="sxs-lookup"><span data-stu-id="6e413-143">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2013 Express Development Tool.</span></span>

<span data-ttu-id="6e413-144">In questa sezione viene illustrato come creare un'applicazione ASP.NET MVC 5, aggiungere la libreria di SignalR e creare l'applicazione di chat.</span><span class="sxs-lookup"><span data-stu-id="6e413-144">This section shows how to create an ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="6e413-145">In Visual Studio, creare un'applicazione c# ASP.NET destinata a .NET Framework 4.5, denominarlo SignalRChat e fare clic su OK.</span><span class="sxs-lookup"><span data-stu-id="6e413-145">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![Creazione di web](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. <span data-ttu-id="6e413-147">Nel `New ASP.NET Project` finestra di dialogo e selezionare **MVC**, fare clic su **Modifica autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="6e413-147">In the `New ASP.NET Project` dialog, and select **MVC**, and click **Change Authentication**.</span></span>

    ![Creazione di web](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. <span data-ttu-id="6e413-149">Selezionare **Nessuna autenticazione** nel **Modifica autenticazione** finestra di dialogo e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="6e413-149">Select **No Authentication** in the **Change Authentication** dialog, and click **OK**.</span></span>

    ![Seleziona Nessuna autenticazione](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > <span data-ttu-id="6e413-151">Se si seleziona un provider di autenticazione diversi per l'applicazione, un `Startup.cs` classe verrà creata automaticamente; non è necessario creare il proprio `Startup.cs` classe nel passaggio 10 seguente.</span><span class="sxs-lookup"><span data-stu-id="6e413-151">If you select a different authentication provider for your application, a `Startup.cs` class will be created for you; you will not need to create your own `Startup.cs` class in step 10 below.</span></span>
4. <span data-ttu-id="6e413-152">Fare clic su **OK** nel **nuovo progetto ASP.NET** finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="6e413-152">Click **OK** in the **New ASP.NET Project** dialog.</span></span>
5. <span data-ttu-id="6e413-153">Aprire il **strumenti | Gestione pacchetti libreria | Console di gestione pacchetti** ed eseguire il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="6e413-153">Open the **Tools | Library Package Manager | Package Manager Console** and run the following command.</span></span> <span data-ttu-id="6e413-154">Questo passaggio viene aggiunta al progetto un set di file di script e i riferimenti ad assembly che abilitano funzionalità SignalR.</span><span class="sxs-lookup"><span data-stu-id="6e413-154">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

    `install-package Microsoft.AspNet.SignalR`
6. <span data-ttu-id="6e413-155">Nelle **Esplora soluzioni**, espandere la cartella degli script.</span><span class="sxs-lookup"><span data-stu-id="6e413-155">In **Solution Explorer**, expand the Scripts folder.</span></span> <span data-ttu-id="6e413-156">Si noti che le librerie di script per SignalR siano stati aggiunti al progetto.</span><span class="sxs-lookup"><span data-stu-id="6e413-156">Note that script libraries for SignalR have been added to the project.</span></span>

    ![Cartella degli script](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. <span data-ttu-id="6e413-158">Nelle **Esplora soluzioni**, fare clic sul progetto, selezionare **Add | Nuova cartella**, e aggiungere una nuova cartella denominata **hub**.</span><span class="sxs-lookup"><span data-stu-id="6e413-158">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
8. <span data-ttu-id="6e413-159">Fare doppio clic il **hub** cartella, fare clic su **Add | Nuovo elemento**, selezionare il **Visual c# | Web | SignalR** nodo il **Installed** riquadro, selezionare **classe tramite SignalR Hub (v2)** nel riquadro centrale e creare un nuovo hub denominato **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="6e413-159">Right-click the **Hubs** folder, click **Add | New Item**, select the **Visual C# | Web | SignalR** node in the **Installed** pane, select **SignalR Hub Class (v2)** from the center pane, and create a new hub named **ChatHub.cs**.</span></span> <span data-ttu-id="6e413-160">Si userà questa classe come hub SignalR server che invia messaggi a tutti i client.</span><span class="sxs-lookup"><span data-stu-id="6e413-160">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

    ![Crea nuovo hub](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. <span data-ttu-id="6e413-162">Sostituire il codice nel **ChatHub** classe con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="6e413-162">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. <span data-ttu-id="6e413-163">Creare una nuova classe denominata Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="6e413-163">Create a new class called Startup.cs.</span></span> <span data-ttu-id="6e413-164">Modificare il contenuto del file come segue.</span><span class="sxs-lookup"><span data-stu-id="6e413-164">Change the contents of the file to the following.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. <span data-ttu-id="6e413-165">Modificare il `HomeController` trovata nella classe **HomeController** e aggiungere il metodo seguente alla classe.</span><span class="sxs-lookup"><span data-stu-id="6e413-165">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="6e413-166">Questo metodo restituisce il **Chat** vista in cui verrà creato in un passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="6e413-166">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. <span data-ttu-id="6e413-167">Fare doppio clic il **Views/Home** cartella e selezionare **Aggiungi... | Visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="6e413-167">Right-click the **Views/Home** folder, and select **Add... | View**.</span></span>
13. <span data-ttu-id="6e413-168">Nel **Aggiungi visualizzazione** finestra di dialogo nome alla nuova visualizzazione **Chat**.</span><span class="sxs-lookup"><span data-stu-id="6e413-168">In the **Add View** dialog, name the new view **Chat**.</span></span>

    ![Aggiungere una vista](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. <span data-ttu-id="6e413-170">Sostituire il contenuto del **Chat.cshtml** con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="6e413-170">Replace the contents of **Chat.cshtml** with the following code.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="6e413-171">Quando si aggiungono SignalR e altre librerie di script al progetto di Visual Studio, la gestione dei pacchetti potrebbe installare una versione del file script SignalR più recenti rispetto a quella illustrata in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="6e413-171">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install a version of the SignalR script file that is more recent than the version shown in this topic.</span></span> <span data-ttu-id="6e413-172">Assicurarsi che il riferimento allo script nel codice corrisponda alla versione della libreria di script installata nel progetto.</span><span class="sxs-lookup"><span data-stu-id="6e413-172">Make sure that the script reference in your code matches the version of the script library installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. <span data-ttu-id="6e413-173">**Salva tutto** per il progetto.</span><span class="sxs-lookup"><span data-stu-id="6e413-173">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="6e413-174">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="6e413-174">Run the Sample</span></span>

1. <span data-ttu-id="6e413-175">Premere F5 per eseguire il progetto in modalità di debug.</span><span class="sxs-lookup"><span data-stu-id="6e413-175">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="6e413-176">Nella riga dell'indirizzo del browser, accodare **/home/chat** all'URL della pagina predefinita per il progetto.</span><span class="sxs-lookup"><span data-stu-id="6e413-176">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="6e413-177">La pagina di Chat viene caricata in un'istanza del browser e richiede un nome utente.</span><span class="sxs-lookup"><span data-stu-id="6e413-177">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![Immettere il nome utente](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. <span data-ttu-id="6e413-179">Immettere un nome utente.</span><span class="sxs-lookup"><span data-stu-id="6e413-179">Enter a user name.</span></span>
4. <span data-ttu-id="6e413-180">Copiare l'URL dalla riga dell'indirizzo del browser e usarlo per aprire due altre istanze del browser.</span><span class="sxs-lookup"><span data-stu-id="6e413-180">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="6e413-181">In ogni istanza del browser immettere un nome utente univoco.</span><span class="sxs-lookup"><span data-stu-id="6e413-181">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="6e413-182">In ogni istanza del browser, aggiungere un commento e fare clic su **inviare**.</span><span class="sxs-lookup"><span data-stu-id="6e413-182">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="6e413-183">I commenti deve essere visualizzato in tutte le istanze del browser.</span><span class="sxs-lookup"><span data-stu-id="6e413-183">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6e413-184">Questa applicazione di chat semplice non gestisce il contesto di discussione sul server.</span><span class="sxs-lookup"><span data-stu-id="6e413-184">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="6e413-185">L'hub trasmette i commenti per tutti gli utenti correnti.</span><span class="sxs-lookup"><span data-stu-id="6e413-185">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="6e413-186">Gli utenti che partecipano alla chat in un secondo momento visualizzeranno messaggi aggiunti dal momento in cui che questi vengono aggiunti.</span><span class="sxs-lookup"><span data-stu-id="6e413-186">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="6e413-187">Lo screenshot seguente mostra l'applicazione di chat in esecuzione in un browser.</span><span class="sxs-lookup"><span data-stu-id="6e413-187">The following screen shot shows the chat application running in a browser.</span></span>

    ![Browser di chat](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. <span data-ttu-id="6e413-189">Nelle **Esplora soluzioni**, esaminare le **documenti Script** nodo per l'applicazione in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="6e413-189">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="6e413-190">Se si usa Internet Explorer come browser, questo nodo è visibile in modalità di debug.</span><span class="sxs-lookup"><span data-stu-id="6e413-190">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="6e413-191">È presente un file di script denominato **hub** che la libreria di SignalR genera in modo dinamico in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="6e413-191">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="6e413-192">Questo file consente di gestire la comunicazione tra script jQuery e codice lato server.</span><span class="sxs-lookup"><span data-stu-id="6e413-192">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="6e413-193">Se si utilizza un browser diverso da Internet Explorer, è anche possibile accedere dinamica **hub** file passando ad esso direttamente, ad esempio http://mywebsite/signalr/hubs.</span><span class="sxs-lookup"><span data-stu-id="6e413-193">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="6e413-194">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="6e413-194">Examine the Code</span></span>

<span data-ttu-id="6e413-195">L'applicazione di chat SignalR illustra due attività di sviluppo di SignalR base: creazione di un hub come oggetto di coordinamento principale nel server e usando la libreria jQuery SignalR per inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="6e413-195">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="6e413-196">Hub SignalR</span><span class="sxs-lookup"><span data-stu-id="6e413-196">SignalR Hubs</span></span>

<span data-ttu-id="6e413-197">Nell'esempio di codice le **ChatHub** deriva dalla classe la **Microsoft.AspNet.SignalR.Hub** classe.</span><span class="sxs-lookup"><span data-stu-id="6e413-197">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="6e413-198">Derivazione dal **Hub** classe è un modo utile per compilare un'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="6e413-198">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="6e413-199">È possibile creare metodi pubblici nella classe dell'hub e quindi accedere a tali metodi chiamandole dagli script in una pagina web.</span><span class="sxs-lookup"><span data-stu-id="6e413-199">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="6e413-200">Nel codice di chat, i client chiamano il **ChatHub.Send** metodo per inviare un nuovo messaggio.</span><span class="sxs-lookup"><span data-stu-id="6e413-200">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="6e413-201">L'hub, a sua volta invia il messaggio a tutti i client chiamando **Clients.All.addNewMessageToPage**.</span><span class="sxs-lookup"><span data-stu-id="6e413-201">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="6e413-202">Il **inviare** metodo illustra alcuni concetti di hub:</span><span class="sxs-lookup"><span data-stu-id="6e413-202">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="6e413-203">Dichiarare i metodi pubblici in un hub in modo che i client possono chiamare li.</span><span class="sxs-lookup"><span data-stu-id="6e413-203">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="6e413-204">Usare la **Microsoft.AspNet.SignalR.Hub.Clients** proprietà a cui accedere tutti i client connessi a questo hub.</span><span class="sxs-lookup"><span data-stu-id="6e413-204">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="6e413-205">Chiamare una funzione nel client (ad esempio il `addNewMessageToPage` (funzione)) per aggiornare i client.</span><span class="sxs-lookup"><span data-stu-id="6e413-205">Call a function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="6e413-206">SignalR e jQuery</span><span class="sxs-lookup"><span data-stu-id="6e413-206">SignalR and jQuery</span></span>

<span data-ttu-id="6e413-207">Il **Chat.cshtml** Visualizza file nell'esempio di codice illustra come usare la libreria jQuery SignalR per comunicare con un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="6e413-207">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="6e413-208">Le attività di base nel codice siano creando un riferimento per il proxy generato automaticamente per l'hub, dichiara una funzione che il server può chiamare per contenuto push ai client e l'avvio di una connessione per inviare messaggi all'hub.</span><span class="sxs-lookup"><span data-stu-id="6e413-208">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="6e413-209">Il codice seguente dichiara un riferimento a un proxy di hub.</span><span class="sxs-lookup"><span data-stu-id="6e413-209">The following code declares a reference to a hub proxy.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="6e413-210">In JavaScript il riferimento alla classe server e i relativi membri è in maiuscole/minuscole camel.</span><span class="sxs-lookup"><span data-stu-id="6e413-210">In JavaScript the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="6e413-211">Nell'esempio di codice fa riferimento il codice c# **ChatHub** classi in JavaScript come **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="6e413-211">The code sample references the C# **ChatHub** class in JavaScript as **chatHub**.</span></span> <span data-ttu-id="6e413-212">Se si desidera fare riferimento il `ChatHub` classe in jQuery con convenzionale convenzione Pascal maiuscole e minuscole come si farebbe in c#, modificare il file di classe ChatHub.cs.</span><span class="sxs-lookup"><span data-stu-id="6e413-212">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="6e413-213">Aggiungere un `using` istruzione a cui fare riferimento il `Microsoft.AspNet.SignalR.Hubs` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="6e413-213">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="6e413-214">Quindi aggiungere il `HubName` dell'attributo per il `ChatHub` classe, ad esempio `[HubName("ChatHub")]`.</span><span class="sxs-lookup"><span data-stu-id="6e413-214">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="6e413-215">Infine, aggiornare il riferimento jQuery il `ChatHub` classe.</span><span class="sxs-lookup"><span data-stu-id="6e413-215">Finally, update your jQuery reference to the `ChatHub` class.</span></span>


<span data-ttu-id="6e413-216">Il codice seguente viene illustrato come creare una funzione di callback nello script.</span><span class="sxs-lookup"><span data-stu-id="6e413-216">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="6e413-217">La classe dell'hub sul server chiama questa funzione per eseguire il push degli aggiornamenti di contenuto a ogni client.</span><span class="sxs-lookup"><span data-stu-id="6e413-217">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="6e413-218">La chiamata facoltativa al `htmlEncode` funzione Mostra un modo per HTML codifica il contenuto del messaggio prima di visualizzarlo nella pagina, come un modo per impedire attacchi script injection.</span><span class="sxs-lookup"><span data-stu-id="6e413-218">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="6e413-219">Il codice seguente viene illustrato come aprire una connessione con l'hub.</span><span class="sxs-lookup"><span data-stu-id="6e413-219">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="6e413-220">Il codice avvia la connessione e quindi lo si passa una funzione per gestire l'evento click sulle **inviare** pulsante nella pagina della Chat.</span><span class="sxs-lookup"><span data-stu-id="6e413-220">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="6e413-221">Questo approccio assicura che la connessione viene stabilita prima esecuzione del gestore dell'evento.</span><span class="sxs-lookup"><span data-stu-id="6e413-221">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="6e413-222">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="6e413-222">Next Steps</span></span>

<span data-ttu-id="6e413-223">Si è appreso che SignalR è un framework per la creazione di applicazioni web in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="6e413-223">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="6e413-224">Si è appreso anche diverse attività di sviluppo di SignalR: come aggiungere SignalR a un'applicazione ASP.NET, come creare una classe di hub e come inviare e ricevere messaggi dall'hub.</span><span class="sxs-lookup"><span data-stu-id="6e413-224">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="6e413-225">Per una procedura dettagliata su come distribuire l'applicazione di esempio SignalR in Azure, vedere [uso di SignalR con App Web nel servizio App di Azure](../deployment/using-signalr-with-azure-web-sites.md).</span><span class="sxs-lookup"><span data-stu-id="6e413-225">For a walkthrough on how to deploy the sample SignalR application to Azure, see [Using SignalR with Web Apps in Azure App Service](../deployment/using-signalr-with-azure-web-sites.md).</span></span> <span data-ttu-id="6e413-226">Per informazioni dettagliate su come distribuire un progetto web Visual Studio per un sito Web di Microsoft Azure, vedere [creare un'app web ASP.NET in servizio App di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="6e413-226">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>

<span data-ttu-id="6e413-227">Per informazioni su concetti più avanzati sviluppi in materia di SignalR, visitare i siti seguenti per il codice sorgente SignalR e risorse:</span><span class="sxs-lookup"><span data-stu-id="6e413-227">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="6e413-228">Progetto di SignalR</span><span class="sxs-lookup"><span data-stu-id="6e413-228">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="6e413-229">SignalR Github ed esempi</span><span class="sxs-lookup"><span data-stu-id="6e413-229">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="6e413-230">Wiki di SignalR</span><span class="sxs-lookup"><span data-stu-id="6e413-230">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
