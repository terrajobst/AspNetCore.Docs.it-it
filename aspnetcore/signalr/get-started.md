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
ms.openlocfilehash: c71d98f86c15a4c6fbbe400f912123419b4ad076
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252204"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="ae0b6-103">Introduzione a SignalR su ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ae0b6-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="ae0b6-104">[Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="ae0b6-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="ae0b6-105">In questa esercitazione illustra le nozioni di base della creazione di un'applicazione in tempo reale mediante SignalR per ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Soluzione](get-started/_static/signalr-get-started-finished.png)

<span data-ttu-id="ae0b6-107">Questa esercitazione illustra le attività di sviluppo SignalR seguenti:</span><span class="sxs-lookup"><span data-stu-id="ae0b6-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ae0b6-108">Creare un SignalR nell'applicazione web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="ae0b6-109">Creare un hub di SignalR per eseguire il push del contenuto ai client.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="ae0b6-110">Modificare il `Startup` classe e configurare l'app.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="ae0b6-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ae0b6-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="ae0b6-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="ae0b6-112">Prerequisites</span></span>

<span data-ttu-id="ae0b6-113">Installare il software seguente:</span><span class="sxs-lookup"><span data-stu-id="ae0b6-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ae0b6-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ae0b6-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="ae0b6-115">.NET core SDK 2.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="ae0b6-115">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="ae0b6-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.7 o versione successiva con il **sviluppo web ASP.NET e** carico di lavoro</span><span class="sxs-lookup"><span data-stu-id="ae0b6-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="ae0b6-117">npm</span><span class="sxs-lookup"><span data-stu-id="ae0b6-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ae0b6-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ae0b6-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="ae0b6-119">.NET core SDK 2.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="ae0b6-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="ae0b6-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ae0b6-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="ae0b6-121">Linguaggio c# per il codice di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ae0b6-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="ae0b6-122">npm</span><span class="sxs-lookup"><span data-stu-id="ae0b6-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="ae0b6-123">Creare un progetto ASP.NET di base che ospita i server e client SignalR</span><span class="sxs-lookup"><span data-stu-id="ae0b6-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ae0b6-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ae0b6-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="ae0b6-125">Utilizzare il **File** > **nuovo progetto** menu opzione **applicazione Web di ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="ae0b6-126">Denominare il progetto *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-126">Name the project *SignalRChat*.</span></span>

   ![Finestra di dialogo Nuovo progetto in Visual Studio](get-started/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="ae0b6-128">Selezionare **applicazione Web** per creare un progetto utilizzando le pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="ae0b6-129">Selezionare quindi **OK**.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-129">Then select **OK**.</span></span> <span data-ttu-id="ae0b6-130">Assicurarsi che **ASP.NET Core 2.1** è selezionata dal selettore del framework, se SignalR viene eseguito nelle versioni precedenti di .NET.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Finestra di dialogo Nuovo progetto in Visual Studio](get-started/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="ae0b6-132">Visual Studio include il `Microsoft.AspNetCore.SignalR` pacchetto contenente le relative librerie server come parte della relativa **applicazione Web di ASP.NET Core** modello.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="ae0b6-133">Tuttavia, la libreria client JavaScript per SignalR deve essere installata utilizzando *npm*.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="ae0b6-134">Eseguire i comandi seguenti **Console di gestione pacchetti** finestra, dalla radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="ae0b6-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```     

4. <span data-ttu-id="ae0b6-135">Crea una nuova cartella denominata "signalr" all'interno di *lib* cartella nel progetto.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-135">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="ae0b6-136">Copia il *signalr.js* file dalla *node_modules\\ @aspnet\signalr\dist\browser*  in questa cartella.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-136">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ae0b6-137">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ae0b6-137">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="ae0b6-138">Dal **Terminal integrata**, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="ae0b6-138">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="ae0b6-139">Installare la libreria client JavaScript usando *npm*.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-139">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="ae0b6-140">Crea una nuova cartella denominata "signalr" all'interno di *lib* cartella nel progetto.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-140">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="ae0b6-141">Copia il *signalr.js* file dalla *node_modules\\ @aspnet\signalr\dist\browser*  in questa cartella.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-141">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="ae0b6-142">Creazione dell'Hub di SignalR</span><span class="sxs-lookup"><span data-stu-id="ae0b6-142">Create the SignalR Hub</span></span>

<span data-ttu-id="ae0b6-143">Un hub è una classe che funge da una pipeline di alto livello che consente al client e server chiamare i metodi tra loro.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-143">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ae0b6-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ae0b6-144">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="ae0b6-145">Aggiungere una classe al progetto scegliendo **File** > **New** > **File** e selezionando **classe Visual c#**.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-145">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="ae0b6-146">Denominare il file *ChatHub*.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-146">Name the file *ChatHub*.</span></span> 

2. <span data-ttu-id="ae0b6-147">Ereditare da `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-147">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="ae0b6-148">La `Hub` classe contiene proprietà ed eventi per la gestione delle connessioni e gruppi, nonché inviare e ricevere dati.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-148">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="ae0b6-149">Creare il `SendMessage` metodo che invia un messaggio a tutti i client connessi chat.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-149">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="ae0b6-150">Si noti restituisce un [attività](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), in quanto SignalR è asincrono.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-150">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="ae0b6-151">Codice asincrono è scalabile.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-151">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ae0b6-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ae0b6-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="ae0b6-153">Aprire la *SignalRChat* cartella in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-153">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="ae0b6-154">Aggiungere una classe al progetto selezionando **File** > **nuovo File** dal menu di scelta.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-154">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="ae0b6-155">Ereditare da `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-155">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="ae0b6-156">Il `Hub` classe contiene proprietà ed eventi per la gestione delle connessioni e gruppi, nonché inviare e ricevere i dati ai client.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-156">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="ae0b6-157">Aggiungere un metodo `SendMessage` alla classe.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-157">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="ae0b6-158">Il `SendMessage` metodo invia un messaggio a tutti i client connessi chat.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-158">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="ae0b6-159">Si noti restituisce un [attività](/dotnet/api/system.threading.tasks.task), in quanto SignalR è asincrono.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-159">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="ae0b6-160">Codice asincrono è scalabile.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-160">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](get-started/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="ae0b6-161">Configurare il progetto per l'utilizzo di SignalR</span><span class="sxs-lookup"><span data-stu-id="ae0b6-161">Configure the project to use SignalR</span></span>

<span data-ttu-id="ae0b6-162">Il server di SignalR deve essere configurato in modo da informarlo che può per passare richieste a SignalR.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-162">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="ae0b6-163">Per configurare un progetto SignalR, modificare il progetto `Startup.ConfigureServices` metodo.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-163">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="ae0b6-164">`services.AddSignalR` Aggiunge SignalR come parte di [middleware](xref:fundamentals/middleware/index) pipeline.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-164">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="ae0b6-165">Configurare le route per l'hub utilizzando `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-165">Configure routes to your hubs using `UseSignalR`.</span></span>


   [!code-csharp[Startup](get-started/sample/Startup.cs?highlight=37,57-60)]


## <a name="create-the-signalr-client-code"></a><span data-ttu-id="ae0b6-166">Creare il codice client SignalR</span><span class="sxs-lookup"><span data-stu-id="ae0b6-166">Create the SignalR client code</span></span>

1. <span data-ttu-id="ae0b6-167">Aggiungere un file JavaScript, denominato *chat.js*, per il *wwwroot\js* cartella.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-167">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="ae0b6-168">Aggiungere il codice seguente al file:</span><span class="sxs-lookup"><span data-stu-id="ae0b6-168">Add the following code to it:</span></span>

   [!code-javascript[Index](get-started/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="ae0b6-169">Sostituire il contenuto di *Pages\Index.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ae0b6-169">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](get-started/sample/Pages/Index.cshtml)]

   <span data-ttu-id="ae0b6-170">HTML precedente Visualizza nome e i campi dei messaggi e un pulsante di invio.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-170">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="ae0b6-171">Notare i riferimenti a script nella parte inferiore: un riferimento a SignalR e *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-171">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>


## <a name="run-the-app"></a><span data-ttu-id="ae0b6-172">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="ae0b6-172">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ae0b6-173">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ae0b6-173">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="ae0b6-174">Selezionare **Debug** > **Avvia senza eseguire debug** per avviare un browser e caricare il sito Web in locale.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-174">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="ae0b6-175">Copiare l'URL dalla barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-175">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="ae0b6-176">Aprire un'altra istanza del browser (qualsiasi browser) e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-176">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="ae0b6-177">Scegliere un browser e immettere un nome e un messaggio, fare clic su di **inviare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-177">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="ae0b6-178">Il nome e il messaggio vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-178">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ae0b6-179">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ae0b6-179">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="ae0b6-180">Premere **Debug** (F5) per compilare ed eseguire il programma.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-180">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="ae0b6-181">Eseguire il programma apre una finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-181">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="ae0b6-182">Aprire un'altra finestra del browser e caricare il sito Web in locale in essa.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-182">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="ae0b6-183">Scegliere un browser e immettere un nome e un messaggio, fare clic su di **inviare** pulsante.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-183">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="ae0b6-184">Il nome e il messaggio vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="ae0b6-184">The name and message are displayed on both pages instantly.</span></span>

---

  ![Soluzione](get-started/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="ae0b6-186">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="ae0b6-186">Related resources</span></span>

[<span data-ttu-id="ae0b6-187">Introduzione a ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="ae0b6-187">Introduction to ASP.NET Core SignalR</span></span>](introduction.md)
