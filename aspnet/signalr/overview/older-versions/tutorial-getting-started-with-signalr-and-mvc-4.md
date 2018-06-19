---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Esercitazione: Introduzione a SignalR 1. x e MVC 4 | Documenti Microsoft'
author: pfletcher
description: Utilizzare ASP.NET SignalR e ASP.NET MVC 4 per creare un'applicazione di chat in tempo reale.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/29/2013
ms.topic: article
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 1ae330be5caf00c3cac7451f326398c0958538af
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873719"
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a><span data-ttu-id="b145c-103">Esercitazione: Introduzione a SignalR 1. x e MVC 4</span><span class="sxs-lookup"><span data-stu-id="b145c-103">Tutorial: Getting Started with SignalR 1.x and MVC 4</span></span>
====================
<span data-ttu-id="b145c-104">dal [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="b145c-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

> <span data-ttu-id="b145c-105">In questa esercitazione viene illustrato come utilizzare ASP.NET SignalR per creare un'applicazione di chat in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="b145c-105">This tutorial shows how to use ASP.NET SignalR to create a real-time chat application.</span></span> <span data-ttu-id="b145c-106">Verrà aggiungere SignalR a un'applicazione MVC 4 e creare una vista di chat per inviare e visualizzare i messaggi.</span><span class="sxs-lookup"><span data-stu-id="b145c-106">You will add SignalR to an MVC 4 application and create a chat view to send and display messages.</span></span>


## <a name="overview"></a><span data-ttu-id="b145c-107">Panoramica</span><span class="sxs-lookup"><span data-stu-id="b145c-107">Overview</span></span>

<span data-ttu-id="b145c-108">Questa esercitazione viene illustrato lo sviluppo di applicazioni web in tempo reale con ASP.NET SignalR e ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b145c-108">This tutorial introduces you to real-time web application development with ASP.NET SignalR and ASP.NET MVC 4.</span></span> <span data-ttu-id="b145c-109">L'esercitazione Usa lo stesso codice applicazione chat di [esercitazione introduttiva SignalR](tutorial-getting-started-with-signalr.md), ma viene illustrato come aggiungerlo a un'applicazione MVC 4 in base al modello di Internet.</span><span class="sxs-lookup"><span data-stu-id="b145c-109">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 4 application based on the Internet template.</span></span>

<span data-ttu-id="b145c-110">In questo argomento verranno illustrate le attività di sviluppo SignalR seguenti:</span><span class="sxs-lookup"><span data-stu-id="b145c-110">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="b145c-111">Aggiunta della libreria di SignalR a un'applicazione MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b145c-111">Adding the SignalR library to an MVC 4 application.</span></span>
- <span data-ttu-id="b145c-112">Creazione di una classe di hub per eseguire il push del contenuto ai client.</span><span class="sxs-lookup"><span data-stu-id="b145c-112">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="b145c-113">Utilizzando la libreria jQuery SignalR in una pagina web per inviare messaggi e visualizzare gli aggiornamenti dall'hub.</span><span class="sxs-lookup"><span data-stu-id="b145c-113">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="b145c-114">La schermata seguente viene illustrata l'applicazione di chat completata in esecuzione in un browser.</span><span class="sxs-lookup"><span data-stu-id="b145c-114">The following screen shot shows the completed chat application running in a browser.</span></span>

![Istanze di chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

<span data-ttu-id="b145c-116">Nelle sezioni:</span><span class="sxs-lookup"><span data-stu-id="b145c-116">Sections:</span></span>

- [<span data-ttu-id="b145c-117">Impostare il progetto</span><span class="sxs-lookup"><span data-stu-id="b145c-117">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="b145c-118">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="b145c-118">Run the Sample</span></span>](#run)
- [<span data-ttu-id="b145c-119">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="b145c-119">Examine the Code</span></span>](#code)
- [<span data-ttu-id="b145c-120">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="b145c-120">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="b145c-121">Impostare il progetto</span><span class="sxs-lookup"><span data-stu-id="b145c-121">Set up the Project</span></span>

<span data-ttu-id="b145c-122">Prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="b145c-122">Prerequisites:</span></span>

- <span data-ttu-id="b145c-123">Visual Studio 2010 SP1, Visual Studio 2012 o Visual Studio Express 2012.</span><span class="sxs-lookup"><span data-stu-id="b145c-123">Visual Studio 2010 SP1, Visual Studio 2012, or Visual Studio 2012 Express.</span></span> <span data-ttu-id="b145c-124">Se non si dispone di Visual Studio, vedere [Scarica ASP.NET](https://www.asp.net/downloads) per ottenere il libero Visual Studio 2012 Express strumento di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="b145c-124">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="b145c-125">Per Visual Studio 2010, installare [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span><span class="sxs-lookup"><span data-stu-id="b145c-125">For Visual Studio 2010, install [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span></span>

<span data-ttu-id="b145c-126">In questa sezione viene illustrato come creare un'applicazione ASP.NET MVC 4, aggiungere la libreria SignalR e creare l'applicazione di chat.</span><span class="sxs-lookup"><span data-stu-id="b145c-126">This section shows how to create an ASP.NET MVC 4 application, add the SignalR library, and create the chat application.</span></span>

1. 1. <span data-ttu-id="b145c-127">In Visual Studio crea un'applicazione ASP.NET MVC 4, denominarla SignalRChat e fare clic su OK.</span><span class="sxs-lookup"><span data-stu-id="b145c-127">In Visual Studio create an ASP.NET MVC 4 application, name it SignalRChat, and click OK.</span></span>

        > [!NOTE]
        > <span data-ttu-id="b145c-128">In Visual Studio 2010, selezionare **.NET Framework 4** nel controllo elenco a discesa della versione di Framework.</span><span class="sxs-lookup"><span data-stu-id="b145c-128">In VS 2010, select **.NET Framework 4** in the Framework version dropdown control.</span></span> <span data-ttu-id="b145c-129">Codice di SignalR viene eseguito nelle versioni di .NET Framework 4 e 4.5.</span><span class="sxs-lookup"><span data-stu-id="b145c-129">SignalR code runs on .NET Framework versions 4 and 4.5.</span></span>

        ![Creare web mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. <span data-ttu-id="b145c-131">Selezionare il modello di applicazione Internet, deselezionare l'opzione **creare un progetto di unit test**, fare clic su OK.</span><span class="sxs-lookup"><span data-stu-id="b145c-131">Select the Internet Application template, clear the option to **Create a unit test project**, and click OK.</span></span>

         ![Creare il sito internet mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. <span data-ttu-id="b145c-133">Aprire il **strumenti | Gestione pacchetti libreria | Console di gestione pacchetti** ed eseguire il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="b145c-133">Open the **Tools | Library Package Manager | Package Manager Console** and run the following command.</span></span> <span data-ttu-id="b145c-134">Questo passaggio viene aggiunto al progetto un set di file di script e i riferimenti ad assembly che abilita la funzionalità di SignalR.</span><span class="sxs-lookup"><span data-stu-id="b145c-134">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. <span data-ttu-id="b145c-135">In **Esplora** espandere la cartella degli script.</span><span class="sxs-lookup"><span data-stu-id="b145c-135">In **Solution Explorer** expand the Scripts folder.</span></span> <span data-ttu-id="b145c-136">Si noti che le librerie di script per SignalR siano stati aggiunti al progetto.</span><span class="sxs-lookup"><span data-stu-id="b145c-136">Note that script libraries for SignalR have been added to the project.</span></span>

         ![Riferimenti alla libreria](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. <span data-ttu-id="b145c-138">In **Esplora**, fare clic sul progetto, selezionare **Aggiungi | Nuova cartella**, e aggiungere una nuova cartella denominata **hub**.</span><span class="sxs-lookup"><span data-stu-id="b145c-138">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
      6. <span data-ttu-id="b145c-139">Fare doppio clic su di **hub** cartella, fare clic su **Aggiungi | Classe**e creare una nuova classe c# denominata **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="b145c-139">Right-click the **Hubs** folder, click **Add | Class**, and create a new C# class named **ChatHub.cs**.</span></span> <span data-ttu-id="b145c-140">Utilizzare questa classe come un hub di SignalR server che invia messaggi a tutti i client.</span><span class="sxs-lookup"><span data-stu-id="b145c-140">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

> [!NOTE]
> <span data-ttu-id="b145c-141">Se si utilizza Visual Studio 2012 e aver installato il [aggiornamento ASP.NET e Web strumenti 2012.2](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), è possibile utilizzare il nuovo modello di elemento di SignalR per creare la classe di hub.</span><span class="sxs-lookup"><span data-stu-id="b145c-141">If you use Visual Studio 2012 and have installed the [ASP.NET and Web Tools 2012.2 update](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), you can use the new SignalR item template to create the hub class.</span></span> <span data-ttu-id="b145c-142">A tale scopo, fare doppio clic su di **hub** cartella, fare clic su **Aggiungi | Nuovo elemento**selezionare **classe Hub SignalR (v1)** e il nome della classe **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="b145c-142">To do that, right-click the **Hubs** folder, click **Add | New Item**, select **SignalR Hub Class (v1)**, and name the class **ChatHub.cs**.</span></span>


1. <span data-ttu-id="b145c-143">Sostituire il codice di **ChatHub** classe con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="b145c-143">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. <span data-ttu-id="b145c-144">Aprire il **Global. asax** file per il progetto e aggiungere una chiamata al metodo `RouteTable.Routes.MapHubs();` come prima riga di codice nel `Application_Start` metodo.</span><span class="sxs-lookup"><span data-stu-id="b145c-144">Open the **Global.asax** file for the project, and add a call to the method `RouteTable.Routes.MapHubs();` as the first line of code in the `Application_Start` method.</span></span> <span data-ttu-id="b145c-145">Il codice registra la route predefinita per gli hub SignalR e deve essere chiamato prima di registrare qualsiasi altre route.</span><span class="sxs-lookup"><span data-stu-id="b145c-145">This code registers the default route for SignalR hubs and must be called before you register any other routes.</span></span> <span data-ttu-id="b145c-146">Completato `Application_Start` metodo è simile alla seguente.</span><span class="sxs-lookup"><span data-stu-id="b145c-146">The completed `Application_Start` method looks like the following example.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. <span data-ttu-id="b145c-147">Modificare il `HomeController` classe trovata nel **Controllers/HomeController.cs** e aggiungere il metodo seguente alla classe.</span><span class="sxs-lookup"><span data-stu-id="b145c-147">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="b145c-148">Questo metodo restituisce il **Chat** visualizzazione che verrà creato in un passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="b145c-148">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. <span data-ttu-id="b145c-149">Fare doppio clic all'interno di `Chat` metodo appena creato, quindi fare clic su **Aggiungi visualizzazione** per creare un nuovo file di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="b145c-149">Right-click within the `Chat` method you just created, and click **Add View** to create a new view file.</span></span>
5. <span data-ttu-id="b145c-150">Nel **Aggiungi visualizzazione** finestra di dialogo, verificare che la casella di controllo è selezionata per **utilizza una layout o pagina master** , deselezionare le caselle di controllo e quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="b145c-150">In the **Add View** dialog, make sure the check box is selected to **Use a layout or master page** (clear the other check boxes), and then click **Add**.</span></span>

    ![Aggiungere una vista](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. <span data-ttu-id="b145c-152">Modificare il nuovo file di visualizzazione denominato **Chat.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="b145c-152">Edit the new view file named **Chat.cshtml**.</span></span> <span data-ttu-id="b145c-153">Dopo il &lt;h2&gt; tag, incollare il codice seguente &lt;div&gt; sezione e `@section scripts` blocco di codice nella pagina.</span><span class="sxs-lookup"><span data-stu-id="b145c-153">After the &lt;h2&gt; tag, paste the following &lt;div&gt; section and `@section scripts` code block into the page.</span></span> <span data-ttu-id="b145c-154">Questo script consente la pagina inviare i messaggi di chat e visualizzare i messaggi dal server.</span><span class="sxs-lookup"><span data-stu-id="b145c-154">This script enables the page to send chat messages and display messages from the server.</span></span> <span data-ttu-id="b145c-155">Il codice completo per la visualizzazione di chat viene visualizzato nel blocco di codice seguente.</span><span class="sxs-lookup"><span data-stu-id="b145c-155">The complete code for the chat view appears in the following code block.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="b145c-156">Quando si aggiungono SignalR e altre librerie di script al progetto di Visual Studio, la gestione dei pacchetti potrebbero installare le versioni degli script che sono più recenti rispetto alle versioni illustrate in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="b145c-156">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install versions of the scripts that are more recent than the versions shown in this topic.</span></span> <span data-ttu-id="b145c-157">Assicurarsi che i riferimenti a script nel codice corrispondono alle versioni delle librerie di script installate nel progetto.</span><span class="sxs-lookup"><span data-stu-id="b145c-157">Make sure that the script references in your code match the versions of the script libraries installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. <span data-ttu-id="b145c-158">**Salva tutto** per il progetto.</span><span class="sxs-lookup"><span data-stu-id="b145c-158">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="b145c-159">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="b145c-159">Run the Sample</span></span>

1. <span data-ttu-id="b145c-160">Premere F5 per eseguire il progetto in modalità di debug.</span><span class="sxs-lookup"><span data-stu-id="b145c-160">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="b145c-161">Nella riga dell'indirizzo del browser, aggiungere **/home/chat** all'URL della pagina predefinita per il progetto.</span><span class="sxs-lookup"><span data-stu-id="b145c-161">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="b145c-162">Caricamento della pagina di Chat in un'istanza del browser e richiede un nome utente.</span><span class="sxs-lookup"><span data-stu-id="b145c-162">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![Immettere il nome utente](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. <span data-ttu-id="b145c-164">Immettere un nome utente.</span><span class="sxs-lookup"><span data-stu-id="b145c-164">Enter a user name.</span></span>
4. <span data-ttu-id="b145c-165">Copiare l'URL dalla riga dell'indirizzo del browser e utilizzarla per aprire le due altre istanze del browser.</span><span class="sxs-lookup"><span data-stu-id="b145c-165">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="b145c-166">In ogni istanza del browser, immettere un nome utente univoco.</span><span class="sxs-lookup"><span data-stu-id="b145c-166">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="b145c-167">In ogni istanza del browser, aggiungere un commento e fare clic su **inviare**.</span><span class="sxs-lookup"><span data-stu-id="b145c-167">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="b145c-168">I commenti deve essere visualizzato in tutte le istanze del browser.</span><span class="sxs-lookup"><span data-stu-id="b145c-168">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b145c-169">Questa applicazione di una chat semplice non gestisce il contesto della discussione sul server.</span><span class="sxs-lookup"><span data-stu-id="b145c-169">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="b145c-170">L'hub trasmette i commenti per tutti gli utenti correnti.</span><span class="sxs-lookup"><span data-stu-id="b145c-170">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="b145c-171">Partecipare alla chat in un secondo momento gli utenti visualizzeranno messaggi aggiunti dal momento in cui che si connettono.</span><span class="sxs-lookup"><span data-stu-id="b145c-171">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="b145c-172">La schermata seguente viene illustrata l'applicazione di chat in esecuzione in un browser.</span><span class="sxs-lookup"><span data-stu-id="b145c-172">The following screen shot shows the chat application running in a browser.</span></span>

    ![Browser chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. <span data-ttu-id="b145c-174">In **Esplora**, controllare il **documenti Script** nodo per l'applicazione in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="b145c-174">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="b145c-175">Questo nodo viene visualizzato in modalità di debug, se si utilizza Internet Explorer come browser.</span><span class="sxs-lookup"><span data-stu-id="b145c-175">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="b145c-176">È presente un file script denominato **hub** che la libreria SignalR genera in modo dinamico in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="b145c-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="b145c-177">Questo file consente di gestire la comunicazione tra script jQuery e codice lato server.</span><span class="sxs-lookup"><span data-stu-id="b145c-177">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="b145c-178">Se si utilizza un browser diverso da Internet Explorer, è inoltre possibile accedere dinamica **hub** file tentato di accedere ad esso direttamente, ad esempio http://mywebsite/signalr/hubs.</span><span class="sxs-lookup"><span data-stu-id="b145c-178">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

    ![Script generato hub](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="b145c-180">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="b145c-180">Examine the Code</span></span>

<span data-ttu-id="b145c-181">L'applicazione di chat SignalR illustra due attività di sviluppo SignalR base: creazione di un hub come oggetto di coordinamento principale sul server e utilizza la libreria di jQuery SignalR per inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="b145c-181">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="b145c-182">Hub di SignalR</span><span class="sxs-lookup"><span data-stu-id="b145c-182">SignalR Hubs</span></span>

<span data-ttu-id="b145c-183">Nell'esempio di codice il **ChatHub** deriva dalla classe di **Microsoft.AspNet.SignalR.Hub** classe.</span><span class="sxs-lookup"><span data-stu-id="b145c-183">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="b145c-184">Derivazione da di **Hub** classe è utile per compilare un'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="b145c-184">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="b145c-185">È possibile creare metodi pubblici nella classe hub e quindi accedere a tali metodi mediante chiamata dagli script jQuery in una pagina web.</span><span class="sxs-lookup"><span data-stu-id="b145c-185">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="b145c-186">Nel codice chat, i client chiamano il **ChatHub.Send** metodo per inviare un nuovo messaggio.</span><span class="sxs-lookup"><span data-stu-id="b145c-186">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="b145c-187">L'hub, a sua volta invia il messaggio a tutti i client chiamando **Clients.All.addNewMessageToPage**.</span><span class="sxs-lookup"><span data-stu-id="b145c-187">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="b145c-188">Il **inviare** metodo illustra i diversi concetti di hub:</span><span class="sxs-lookup"><span data-stu-id="b145c-188">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="b145c-189">Dichiarare i metodi pubblici in un hub in modo che i client possono essere richiamati.</span><span class="sxs-lookup"><span data-stu-id="b145c-189">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="b145c-190">Utilizzare il **Microsoft.AspNet.SignalR.Hub.Clients** proprietà per accedere a tutti i client connessi a questo hub.</span><span class="sxs-lookup"><span data-stu-id="b145c-190">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="b145c-191">Chiamare una funzione di jQuery sul client (ad esempio il `addNewMessageToPage` (funzione)) per aggiornare i client.</span><span class="sxs-lookup"><span data-stu-id="b145c-191">Call a jQuery function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="b145c-192">SignalR e jQuery</span><span class="sxs-lookup"><span data-stu-id="b145c-192">SignalR and jQuery</span></span>

<span data-ttu-id="b145c-193">Il **Chat.cshtml** Visualizza file nell'esempio di codice viene illustrato come utilizzare la libreria jQuery SignalR per comunicare con un hub di SignalR.</span><span class="sxs-lookup"><span data-stu-id="b145c-193">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="b145c-194">Attività essenziali nel codice crea un riferimento per il proxy generato automaticamente per l'hub, dichiarazione di una funzione che il server è possibile chiamare per contenuto push ai client e all'avvio di una connessione per inviare messaggi all'hub.</span><span class="sxs-lookup"><span data-stu-id="b145c-194">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="b145c-195">Il codice seguente dichiara un proxy per un hub.</span><span class="sxs-lookup"><span data-stu-id="b145c-195">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="b145c-196">In jQuery il riferimento alla classe del server e i relativi membri è in maiuscole-minuscole camel.</span><span class="sxs-lookup"><span data-stu-id="b145c-196">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="b145c-197">L'esempio di codice fa riferimento a c# **ChatHub** classe jQuery come **chatHub**.</span><span class="sxs-lookup"><span data-stu-id="b145c-197">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span> <span data-ttu-id="b145c-198">Se si desidera fare riferimento il `ChatHub` classe in jQuery con Pascal convenzionale maiuscole e minuscole come si farebbe in c#, modificare il file di classe ChatHub.cs.</span><span class="sxs-lookup"><span data-stu-id="b145c-198">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="b145c-199">Aggiungere un `using` istruzione per fare riferimento il `Microsoft.AspNet.SignalR.Hubs` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="b145c-199">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="b145c-200">Aggiungere quindi il `HubName` attributo la `ChatHub` classe, ad esempio `[HubName("ChatHub")]`.</span><span class="sxs-lookup"><span data-stu-id="b145c-200">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="b145c-201">Infine, aggiornare il riferimento jQuery la `ChatHub` classe.</span><span class="sxs-lookup"><span data-stu-id="b145c-201">Finally, update your jQuery reference to the `ChatHub` class.</span></span>


<span data-ttu-id="b145c-202">Il codice seguente viene illustrato come creare una funzione di callback nello script.</span><span class="sxs-lookup"><span data-stu-id="b145c-202">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="b145c-203">Classe dell'hub sul server chiama questa funzione per inviare gli aggiornamenti del contenuto di ogni client.</span><span class="sxs-lookup"><span data-stu-id="b145c-203">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="b145c-204">La chiamata a facoltativo di `htmlEncode` funzione Mostra un modo per HTML codifica il contenuto del messaggio prima di visualizzarlo nella pagina, come un modo per impedire attacchi script injection.</span><span class="sxs-lookup"><span data-stu-id="b145c-204">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

<span data-ttu-id="b145c-205">Il codice seguente viene illustrato come aprire una connessione con l'hub.</span><span class="sxs-lookup"><span data-stu-id="b145c-205">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="b145c-206">Il codice avvia la connessione e passa a una funzione per gestire l'evento click nel **inviare** nella pagina Chat.</span><span class="sxs-lookup"><span data-stu-id="b145c-206">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="b145c-207">Questo approccio assicura che la connessione viene stabilita prima dell'esecuzione il gestore dell'evento.</span><span class="sxs-lookup"><span data-stu-id="b145c-207">This approach ensures that the connection is established before the event handler executes.</span></span>


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="b145c-208">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="b145c-208">Next Steps</span></span>

<span data-ttu-id="b145c-209">Si è appreso che SignalR è un framework per la creazione di applicazioni web in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="b145c-209">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="b145c-210">È stato inoltre diverse attività di sviluppo SignalR: come aggiungere SignalR a un'applicazione ASP.NET, come creare una classe di hub e come inviare e ricevere messaggi dall'hub.</span><span class="sxs-lookup"><span data-stu-id="b145c-210">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="b145c-211">Per informazioni su concetti più avanzati di sviluppi SignalR, visitare i siti seguenti per il codice sorgente SignalR e risorse:</span><span class="sxs-lookup"><span data-stu-id="b145c-211">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="b145c-212">Progetto SignalR</span><span class="sxs-lookup"><span data-stu-id="b145c-212">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="b145c-213">SignalR Github ed esempi</span><span class="sxs-lookup"><span data-stu-id="b145c-213">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="b145c-214">Wiki di SignalR</span><span class="sxs-lookup"><span data-stu-id="b145c-214">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
