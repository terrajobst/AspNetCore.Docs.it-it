---
title: Introduzione a SignalR su ASP.NET Core
author: rachelappel
description: In questa esercitazione, si crea un'app usando SignalR per ASP.NET Core.
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/16/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: cf120d535c85c7871f5b1f27039018ea2405b9cb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="40b08-103">Introduzione a SignalR su ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="40b08-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="40b08-104">[Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="40b08-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

[!INCLUDE [Version notice](../includes/signalr-version-notice.md)]

<span data-ttu-id="40b08-105">In questa esercitazione illustra le nozioni di base della creazione di un'applicazione in tempo reale mediante SignalR per ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="40b08-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Soluzione](get-started/_static/signalr-get-started-finished.png)

<span data-ttu-id="40b08-107">Questa esercitazione illustra le attività di sviluppo SignalR seguenti:</span><span class="sxs-lookup"><span data-stu-id="40b08-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="40b08-108">Creare un SignalR nell'applicazione web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="40b08-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="40b08-109">Creare un hub di SignalR per eseguire il push del contenuto ai client.</span><span class="sxs-lookup"><span data-stu-id="40b08-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="40b08-110">Modificare il `Startup` classe e configurare l'app.</span><span class="sxs-lookup"><span data-stu-id="40b08-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="40b08-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="40b08-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="40b08-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="40b08-112">Prerequisites</span></span>

<span data-ttu-id="40b08-113">Installare il software seguente:</span><span class="sxs-lookup"><span data-stu-id="40b08-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="40b08-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="40b08-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="40b08-115">[.NET core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) o versione successiva</span><span class="sxs-lookup"><span data-stu-id="40b08-115">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* <span data-ttu-id="40b08-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15,6 o versione successiva con il **sviluppo web ASP.NET e** carico di lavoro</span><span class="sxs-lookup"><span data-stu-id="40b08-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.6 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="40b08-117">npm</span><span class="sxs-lookup"><span data-stu-id="40b08-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="40b08-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="40b08-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="40b08-119">[.NET core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) o versione successiva</span><span class="sxs-lookup"><span data-stu-id="40b08-119">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* [<span data-ttu-id="40b08-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="40b08-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download) 
* [<span data-ttu-id="40b08-121">Linguaggio c# per il codice di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="40b08-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="40b08-122">npm</span><span class="sxs-lookup"><span data-stu-id="40b08-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="40b08-123">Creare un progetto ASP.NET di base che ospita i server e client SignalR</span><span class="sxs-lookup"><span data-stu-id="40b08-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

#### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="40b08-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="40b08-124">Visual Studio</span></span>](#tab/visual-studio/)
1. <span data-ttu-id="40b08-125">Utilizzare il **File** > **nuovo progetto** menu opzione **applicazione Web di ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="40b08-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="40b08-126">Denominare il progetto *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="40b08-126">Name the project *SignalRChat*.</span></span>

   ![Finestra di dialogo Nuovo progetto in Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="40b08-128">Selezionare **applicazione Web** per creare un progetto utilizzando le pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="40b08-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="40b08-129">Selezionare quindi **OK**.</span><span class="sxs-lookup"><span data-stu-id="40b08-129">Then select **OK**.</span></span> <span data-ttu-id="40b08-130">Assicurarsi che **ASP.NET Core 2.1** è selezionata dal selettore del framework, se SignalR viene eseguito nelle versioni precedenti di .NET.</span><span class="sxs-lookup"><span data-stu-id="40b08-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Finestra di dialogo Nuovo progetto in Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

3. <span data-ttu-id="40b08-132">Fare clic sul progetto in **Esplora soluzioni** > **Add** > **nuovo elemento** > **npm File di configurazione** .</span><span class="sxs-lookup"><span data-stu-id="40b08-132">Right-click the project in **Solution Explorer** > **Add** > **New Item** > **npm Configuration File**.</span></span> <span data-ttu-id="40b08-133">Denominare il file *package. JSON*.</span><span class="sxs-lookup"><span data-stu-id="40b08-133">Name the file *package.json*.</span></span>

4. <span data-ttu-id="40b08-134">Eseguire il comando seguente **Console di gestione pacchetti** finestra, dalla radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="40b08-134">Run the following command in the **Package Manager Console** window, from the project root:</span></span>

    ```console
      npm install @aspnet/signalr
    ```
5. <span data-ttu-id="40b08-135">Copia il <em>signalr.js</em> file dalla <em>node_modules\\ @aspnet\signalr\dist\browser</em>  per il <em>wwwroot\lib</em> cartella nel progetto.</span><span class="sxs-lookup"><span data-stu-id="40b08-135">Copy the <em>signalr.js</em> file from <em>node_modules\\@aspnet\signalr\dist\browser</em> to the <em>wwwroot\lib</em> folder in your project.</span></span>

#### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="40b08-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="40b08-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)
1. <span data-ttu-id="40b08-137">Dal **Terminal integrata**, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="40b08-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
      dotnet new razor -o SignalRChat
    ```

2. <span data-ttu-id="40b08-138">Installare la libreria client JavaScript usando *npm*.</span><span class="sxs-lookup"><span data-stu-id="40b08-138">Install the JavaScript client library using *npm*.</span></span>

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

* * *
## <a name="create-the-signalr-hub"></a><span data-ttu-id="40b08-139">Creazione dell'Hub di SignalR</span><span class="sxs-lookup"><span data-stu-id="40b08-139">Create the SignalR Hub</span></span>

<span data-ttu-id="40b08-140">Un hub è una classe che funge da una pipeline di alto livello che consente al client e server chiamare i metodi tra loro.</span><span class="sxs-lookup"><span data-stu-id="40b08-140">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

#### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="40b08-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="40b08-141">Visual Studio</span></span>](#tab/visual-studio/)
1. <span data-ttu-id="40b08-142">Aggiungere una classe al progetto scegliendo **File** > **New** > **File** e selezionando **classe Visual c#**.</span><span class="sxs-lookup"><span data-stu-id="40b08-142">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span>

2. <span data-ttu-id="40b08-143">Ereditare da `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="40b08-143">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="40b08-144">La `Hub` classe contiene proprietà ed eventi per la gestione delle connessioni e gruppi, nonché inviare e ricevere dati.</span><span class="sxs-lookup"><span data-stu-id="40b08-144">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="40b08-145">Creare il `SendMessage` metodo che invia un messaggio a tutti i client connessi chat.</span><span class="sxs-lookup"><span data-stu-id="40b08-145">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="40b08-146">Si noti restituisce un [attività](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task(v=vs.110).aspx), in quanto SignalR è asincrono.</span><span class="sxs-lookup"><span data-stu-id="40b08-146">Notice it returns a [Task](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="40b08-147">Codice asincrono è scalabile.</span><span class="sxs-lookup"><span data-stu-id="40b08-147">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=7-14)]

#### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="40b08-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="40b08-148">Visual Studio Code</span></span>](#tab/visual-studio-code/)
1. <span data-ttu-id="40b08-149">Aprire la *SignalRChat* cartella in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="40b08-149">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="40b08-150">Aggiungere una classe al progetto selezionando **File** > **nuovo File** dal menu di scelta.</span><span class="sxs-lookup"><span data-stu-id="40b08-150">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="40b08-151">Ereditare da `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="40b08-151">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="40b08-152">Il `Hub` classe contiene proprietà ed eventi per la gestione delle connessioni e gruppi, nonché inviare e ricevere i dati ai client.</span><span class="sxs-lookup"><span data-stu-id="40b08-152">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="40b08-153">Aggiungere un metodo `SendMessage` alla classe.</span><span class="sxs-lookup"><span data-stu-id="40b08-153">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="40b08-154">Il `SendMessage` metodo invia un messaggio a tutti i client connessi chat.</span><span class="sxs-lookup"><span data-stu-id="40b08-154">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="40b08-155">Si noti restituisce un [attività](/dotnet/api/system.threading.tasks.task), in quanto SignalR è asincrono.</span><span class="sxs-lookup"><span data-stu-id="40b08-155">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="40b08-156">Codice asincrono è scalabile.</span><span class="sxs-lookup"><span data-stu-id="40b08-156">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=7-14)]

* * *
## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="40b08-157">Configurare il progetto per l'utilizzo di SignalR</span><span class="sxs-lookup"><span data-stu-id="40b08-157">Configure the project to use SignalR</span></span>

<span data-ttu-id="40b08-158">Il server di SignalR deve essere configurato in modo da informarlo che può per passare richieste a SignalR.</span><span class="sxs-lookup"><span data-stu-id="40b08-158">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="40b08-159">Per configurare un progetto SignalR, modificare il progetto `Startup.ConfigureServices` metodo.</span><span class="sxs-lookup"><span data-stu-id="40b08-159">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="40b08-160">`services.AddSignalR` Aggiunge SignalR come parte di [middleware](xref:fundamentals/middleware/index) pipeline.</span><span class="sxs-lookup"><span data-stu-id="40b08-160">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="40b08-161">Configurare le route per l'hub utilizzando `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="40b08-161">Configure routes to your hubs using `UseSignalR`.</span></span>

   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="40b08-162">Creare il codice client SignalR</span><span class="sxs-lookup"><span data-stu-id="40b08-162">Create the SignalR client code</span></span>

1. <span data-ttu-id="40b08-163">Sostituire il contenuto di *Pages\Index.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="40b08-163">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   <span data-ttu-id="40b08-164">HTML precedente Visualizza nome e i campi dei messaggi e un pulsante di invio.</span><span class="sxs-lookup"><span data-stu-id="40b08-164">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="40b08-165">Notare i riferimenti a script nella parte inferiore: un riferimento a SignalR e *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="40b08-165">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

2. <span data-ttu-id="40b08-166">Aggiungere un file JavaScript, denominato *chat.js*, per il *wwwroot\js* cartella.</span><span class="sxs-lookup"><span data-stu-id="40b08-166">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="40b08-167">Aggiungere il codice seguente al file:</span><span class="sxs-lookup"><span data-stu-id="40b08-167">Add the following code to it:</span></span>

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="40b08-168">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="40b08-168">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="40b08-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="40b08-169">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="40b08-170">Selezionare **Debug** > **Avvia senza eseguire debug** per avviare un browser e caricare il sito Web in locale.</span><span class="sxs-lookup"><span data-stu-id="40b08-170">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="40b08-171">Copiare l'URL dalla barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="40b08-171">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="40b08-172">Aprire un'altra istanza del browser (qualsiasi browser) e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="40b08-172">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="40b08-173">Scegliere un browser e immettere un nome e un messaggio, fare clic su di **inviare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="40b08-173">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="40b08-174">Il nome e il messaggio vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="40b08-174">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="40b08-175">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="40b08-175">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="40b08-176">Premere **Debug** (F5) per compilare ed eseguire il programma.</span><span class="sxs-lookup"><span data-stu-id="40b08-176">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="40b08-177">Eseguire il programma apre una finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="40b08-177">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="40b08-178">Aprire un'altra finestra del browser e caricare il sito Web in locale in essa.</span><span class="sxs-lookup"><span data-stu-id="40b08-178">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="40b08-179">Scegliere un browser e immettere un nome e un messaggio, fare clic su di **inviare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="40b08-179">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="40b08-180">Il nome e il messaggio vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="40b08-180">The name and message are displayed on both pages instantly.</span></span>

-----

  ![Soluzione](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="40b08-182">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="40b08-182">Related resources</span></span>

[<span data-ttu-id="40b08-183">Introduzione a ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="40b08-183">Introduction to ASP.NET Core SignalR</span></span>](introduction.md)