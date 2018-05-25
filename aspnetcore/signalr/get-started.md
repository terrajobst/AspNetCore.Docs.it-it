---
title: Introduzione a SignalR su ASP.NET Core
author: rachelappel
description: In questa esercitazione, si crea un'app usando SignalR per ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started
ms.openlocfilehash: fb448f22db5bb624083f07cefaad29d6d58331ef
ms.sourcegitcommit: 300a1127957dcdbce1b6ad79a7b9dc676f571510
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/23/2018
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="0e86f-103">Introduzione a SignalR su ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0e86f-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="0e86f-104">[Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="0e86f-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="0e86f-105">In questa esercitazione illustra le nozioni di base della creazione di un'applicazione in tempo reale mediante SignalR per ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0e86f-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Soluzione](get-started/_static/signalr-get-started-finished.png)

<span data-ttu-id="0e86f-107">Questa esercitazione illustra le attività di sviluppo SignalR seguenti:</span><span class="sxs-lookup"><span data-stu-id="0e86f-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0e86f-108">Creare un SignalR nell'applicazione web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0e86f-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="0e86f-109">Creare un hub di SignalR per eseguire il push del contenuto ai client.</span><span class="sxs-lookup"><span data-stu-id="0e86f-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="0e86f-110">Modificare il `Startup` classe e configurare l'app.</span><span class="sxs-lookup"><span data-stu-id="0e86f-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="0e86f-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0e86f-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="0e86f-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="0e86f-112">Prerequisites</span></span>

<span data-ttu-id="0e86f-113">Installare il software seguente:</span><span class="sxs-lookup"><span data-stu-id="0e86f-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0e86f-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0e86f-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0e86f-115">[.NET core 2.1.0 RC 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-rc1) o versione successiva</span><span class="sxs-lookup"><span data-stu-id="0e86f-115">[.NET Core 2.1.0 RC 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-rc1) or later</span></span>
* <span data-ttu-id="0e86f-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.7 o versione successiva con il **sviluppo web ASP.NET e** carico di lavoro</span><span class="sxs-lookup"><span data-stu-id="0e86f-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="0e86f-117">npm</span><span class="sxs-lookup"><span data-stu-id="0e86f-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0e86f-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0e86f-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0e86f-119">[.NET core 2.1.0 RC 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-rc1) o versione successiva</span><span class="sxs-lookup"><span data-stu-id="0e86f-119">[.NET Core 2.1.0 RC 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-rc1) or later</span></span>
* [<span data-ttu-id="0e86f-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0e86f-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="0e86f-121">Linguaggio c# per il codice di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0e86f-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="0e86f-122">npm</span><span class="sxs-lookup"><span data-stu-id="0e86f-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="0e86f-123">Creare un progetto ASP.NET di base che ospita i server e client SignalR</span><span class="sxs-lookup"><span data-stu-id="0e86f-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0e86f-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0e86f-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="0e86f-125">Utilizzare il **File** > **nuovo progetto** menu opzione **applicazione Web di ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="0e86f-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="0e86f-126">Denominare il progetto *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="0e86f-126">Name the project *SignalRChat*.</span></span>

   ![Finestra di dialogo Nuovo progetto in Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="0e86f-128">Selezionare **applicazione Web** per creare un progetto utilizzando le pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="0e86f-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="0e86f-129">Selezionare quindi **OK**.</span><span class="sxs-lookup"><span data-stu-id="0e86f-129">Then select **OK**.</span></span> <span data-ttu-id="0e86f-130">Assicurarsi che **ASP.NET Core 2.1** è selezionata dal selettore del framework, se SignalR viene eseguito nelle versioni precedenti di .NET.</span><span class="sxs-lookup"><span data-stu-id="0e86f-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Finestra di dialogo Nuovo progetto in Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="0e86f-132">Visual Studio include il `Microsoft.AspNetCore.SignalR` pacchetto contenente le relative librerie server come parte della relativa **applicazione Web di ASP.NET Core** modello.</span><span class="sxs-lookup"><span data-stu-id="0e86f-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="0e86f-133">Tuttavia, la libreria client JavaScript per SignalR deve essere installata utilizzando *npm*.</span><span class="sxs-lookup"><span data-stu-id="0e86f-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="0e86f-134">Eseguire i comandi seguenti **Console di gestione pacchetti** finestra, dalla radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="0e86f-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```     

4. <span data-ttu-id="0e86f-135">Copia il *signalr.js* file dalla *node_modules\\ @aspnet\signalr\dist\browser*  per il *lib* cartella nel progetto.</span><span class="sxs-lookup"><span data-stu-id="0e86f-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *lib* folder in your project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0e86f-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0e86f-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="0e86f-137">Dal **Terminal integrata**, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="0e86f-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new razor -o SignalRChat
    ```

2. <span data-ttu-id="0e86f-138">Installare la libreria client JavaScript usando *npm*.</span><span class="sxs-lookup"><span data-stu-id="0e86f-138">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="0e86f-139">Copia il *signalr.js* file dalla *node_modules\\ @aspnet\signalr\dist\browser*  per il *lib* cartella nel progetto.</span><span class="sxs-lookup"><span data-stu-id="0e86f-139">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *lib* folder in your project.</span></span>

-----

## <a name="create-the-signalr-hub"></a><span data-ttu-id="0e86f-140">Creazione dell'Hub di SignalR</span><span class="sxs-lookup"><span data-stu-id="0e86f-140">Create the SignalR Hub</span></span>

<span data-ttu-id="0e86f-141">Un hub è una classe che funge da una pipeline di alto livello che consente al client e server chiamare i metodi tra loro.</span><span class="sxs-lookup"><span data-stu-id="0e86f-141">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0e86f-142">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0e86f-142">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="0e86f-143">Aggiungere una classe al progetto scegliendo **File** > **New** > **File** e selezionando **classe Visual c#**.</span><span class="sxs-lookup"><span data-stu-id="0e86f-143">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span>

2. <span data-ttu-id="0e86f-144">Ereditare da `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="0e86f-144">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="0e86f-145">La `Hub` classe contiene proprietà ed eventi per la gestione delle connessioni e gruppi, nonché inviare e ricevere dati.</span><span class="sxs-lookup"><span data-stu-id="0e86f-145">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="0e86f-146">Creare il `SendMessage` metodo che invia un messaggio a tutti i client connessi chat.</span><span class="sxs-lookup"><span data-stu-id="0e86f-146">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="0e86f-147">Si noti restituisce un [attività](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), in quanto SignalR è asincrono.</span><span class="sxs-lookup"><span data-stu-id="0e86f-147">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="0e86f-148">Codice asincrono è scalabile.</span><span class="sxs-lookup"><span data-stu-id="0e86f-148">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0e86f-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0e86f-149">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="0e86f-150">Aprire la *SignalRChat* cartella in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="0e86f-150">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="0e86f-151">Aggiungere una classe al progetto selezionando **File** > **nuovo File** dal menu di scelta.</span><span class="sxs-lookup"><span data-stu-id="0e86f-151">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="0e86f-152">Ereditare da `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="0e86f-152">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="0e86f-153">Il `Hub` classe contiene proprietà ed eventi per la gestione delle connessioni e gruppi, nonché inviare e ricevere i dati ai client.</span><span class="sxs-lookup"><span data-stu-id="0e86f-153">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="0e86f-154">Aggiungere un metodo `SendMessage` alla classe.</span><span class="sxs-lookup"><span data-stu-id="0e86f-154">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="0e86f-155">Il `SendMessage` metodo invia un messaggio a tutti i client connessi chat.</span><span class="sxs-lookup"><span data-stu-id="0e86f-155">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="0e86f-156">Si noti restituisce un [attività](/dotnet/api/system.threading.tasks.task), in quanto SignalR è asincrono.</span><span class="sxs-lookup"><span data-stu-id="0e86f-156">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="0e86f-157">Codice asincrono è scalabile.</span><span class="sxs-lookup"><span data-stu-id="0e86f-157">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="0e86f-158">Configurare il progetto per l'utilizzo di SignalR</span><span class="sxs-lookup"><span data-stu-id="0e86f-158">Configure the project to use SignalR</span></span>

<span data-ttu-id="0e86f-159">Il server di SignalR deve essere configurato in modo da informarlo che può per passare richieste a SignalR.</span><span class="sxs-lookup"><span data-stu-id="0e86f-159">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="0e86f-160">Per configurare un progetto SignalR, modificare il progetto `Startup.ConfigureServices` metodo.</span><span class="sxs-lookup"><span data-stu-id="0e86f-160">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="0e86f-161">`services.AddSignalR` Aggiunge SignalR come parte di [middleware](xref:fundamentals/middleware/index) pipeline.</span><span class="sxs-lookup"><span data-stu-id="0e86f-161">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="0e86f-162">Configurare le route per l'hub utilizzando `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="0e86f-162">Configure routes to your hubs using `UseSignalR`.</span></span>


   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=37,57-60)]


## <a name="create-the-signalr-client-code"></a><span data-ttu-id="0e86f-163">Creare il codice client SignalR</span><span class="sxs-lookup"><span data-stu-id="0e86f-163">Create the SignalR client code</span></span>

1. <span data-ttu-id="0e86f-164">Sostituire il contenuto di *Pages\Index.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="0e86f-164">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   <span data-ttu-id="0e86f-165">HTML precedente Visualizza nome e i campi dei messaggi e un pulsante di invio.</span><span class="sxs-lookup"><span data-stu-id="0e86f-165">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="0e86f-166">Notare i riferimenti a script nella parte inferiore: un riferimento a SignalR e *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="0e86f-166">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

2. <span data-ttu-id="0e86f-167">Aggiungere un file JavaScript, denominato *chat.js*, per il *wwwroot\js* cartella.</span><span class="sxs-lookup"><span data-stu-id="0e86f-167">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="0e86f-168">Aggiungere il codice seguente al file:</span><span class="sxs-lookup"><span data-stu-id="0e86f-168">Add the following code to it:</span></span>

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="0e86f-169">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="0e86f-169">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0e86f-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0e86f-170">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="0e86f-171">Selezionare **Debug** > **Avvia senza eseguire debug** per avviare un browser e caricare il sito Web in locale.</span><span class="sxs-lookup"><span data-stu-id="0e86f-171">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="0e86f-172">Copiare l'URL dalla barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="0e86f-172">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="0e86f-173">Aprire un'altra istanza del browser (qualsiasi browser) e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="0e86f-173">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="0e86f-174">Scegliere un browser e immettere un nome e un messaggio, fare clic su di **inviare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="0e86f-174">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="0e86f-175">Il nome e il messaggio vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="0e86f-175">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0e86f-176">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0e86f-176">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="0e86f-177">Premere **Debug** (F5) per compilare ed eseguire il programma.</span><span class="sxs-lookup"><span data-stu-id="0e86f-177">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="0e86f-178">Eseguire il programma apre una finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="0e86f-178">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="0e86f-179">Aprire un'altra finestra del browser e caricare il sito Web in locale in essa.</span><span class="sxs-lookup"><span data-stu-id="0e86f-179">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="0e86f-180">Scegliere un browser e immettere un nome e un messaggio, fare clic su di **inviare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="0e86f-180">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="0e86f-181">Il nome e il messaggio vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="0e86f-181">The name and message are displayed on both pages instantly.</span></span>

---

  ![Soluzione](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="0e86f-183">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="0e86f-183">Related resources</span></span>

[<span data-ttu-id="0e86f-184">Introduzione a ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="0e86f-184">Introduction to ASP.NET Core SignalR</span></span>](introduction.md)
