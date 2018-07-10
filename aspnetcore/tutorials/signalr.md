---
title: Introduzione a SignalR in ASP.NET Core
author: rachelappel
description: In questa esercitazione viene creata un'app usando SignalR per ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 62cef2d6f032caa2f048cfdd49a225d975dad10d
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37033342"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="31858-103">Introduzione a SignalR in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="31858-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="31858-104">[Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="31858-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="31858-105">Questa esercitazione illustra le nozioni di base per compilare un'app in tempo reale con SignalR per ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="31858-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Soluzione](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="31858-107">In questa esercitazione vengono illustrate le attività di sviluppo di SignalR seguenti:</span><span class="sxs-lookup"><span data-stu-id="31858-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="31858-108">Creare un servizio SignalR in un'app Web di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="31858-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="31858-109">Creare un hub SignalR per eseguire il push dei contenuti ai client.</span><span class="sxs-lookup"><span data-stu-id="31858-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="31858-110">Modificare la classe `Startup` e configurare l'app.</span><span class="sxs-lookup"><span data-stu-id="31858-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="31858-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="31858-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="31858-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="31858-112">Prerequisites</span></span>

<span data-ttu-id="31858-113">Installare il software seguente:</span><span class="sxs-lookup"><span data-stu-id="31858-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="31858-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="31858-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="31858-115">.NET Core SDK 2.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="31858-115">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="31858-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) versione 15.7 o successiva con il carico di lavoro **Sviluppo ASP.NET e Web**</span><span class="sxs-lookup"><span data-stu-id="31858-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="31858-117">npm</span><span class="sxs-lookup"><span data-stu-id="31858-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="31858-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="31858-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="31858-119">.NET Core SDK 2.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="31858-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="31858-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="31858-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="31858-121">C# per Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="31858-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="31858-122">npm</span><span class="sxs-lookup"><span data-stu-id="31858-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="31858-123">Creare un progetto ASP.NET Core che ospita il client e il server SignalR</span><span class="sxs-lookup"><span data-stu-id="31858-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="31858-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="31858-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="31858-125">Selezionare l'opzione di menu **File** > **Nuovo progetto** e scegliere **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="31858-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="31858-126">Denominare il progetto *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="31858-126">Name the project *SignalRChat*.</span></span>

   ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="31858-128">Selezionare **Applicazione Web** per creare un progetto usando le Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="31858-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="31858-129">Selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="31858-129">Then select **OK**.</span></span> <span data-ttu-id="31858-130">Assicurarsi che nel selettore del framework sia selezionato **ASP.NET Core 2.1**, anche se SignalR viene eseguito in versioni precedenti di .NET.</span><span class="sxs-lookup"><span data-stu-id="31858-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="31858-132">Visual Studio include il pacchetto `Microsoft.AspNetCore.SignalR` contenente le relative librerie server come parte del modello **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="31858-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="31858-133">Tuttavia, la libreria client JavaScript per SignalR deve essere installata usando *npm*.</span><span class="sxs-lookup"><span data-stu-id="31858-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="31858-134">Eseguire i comandi seguenti nella finestra **Console di gestione pacchetti**, dalla radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="31858-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. <span data-ttu-id="31858-135">Creare una nuova cartella denominata "signalr" all'interno della cartella *lib* nel progetto.</span><span class="sxs-lookup"><span data-stu-id="31858-135">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="31858-136">Copiare il file *signalr.js* da *node_modules\\@aspnet\signalr\dist\browser* in questa cartella.</span><span class="sxs-lookup"><span data-stu-id="31858-136">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="31858-137">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="31858-137">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="31858-138">Da **Terminale integrato** eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="31858-138">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="31858-139">Installare la libreria client JavaScript usando *npm*.</span><span class="sxs-lookup"><span data-stu-id="31858-139">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="31858-140">Creare una nuova cartella denominata "signalr" all'interno della cartella *lib* nel progetto.</span><span class="sxs-lookup"><span data-stu-id="31858-140">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="31858-141">Copiare il file *signalr.js* da *node_modules\\@aspnet\signalr\dist\browser* in questa cartella.</span><span class="sxs-lookup"><span data-stu-id="31858-141">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="31858-142">Creare l'hub di SignalR</span><span class="sxs-lookup"><span data-stu-id="31858-142">Create the SignalR Hub</span></span>

<span data-ttu-id="31858-143">Un hub è una classe che funge da pipeline di alto livello che consente al client e al server di chiamare metodi tra loro.</span><span class="sxs-lookup"><span data-stu-id="31858-143">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="31858-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="31858-144">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="31858-145">Aggiungere una classe al progetto scegliendo **File** > **Nuovo** > **File** e selezionando **Classe di Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="31858-145">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="31858-146">Assegnare alla classe il nome `ChatHub` e al file il nome *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="31858-146">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

2. <span data-ttu-id="31858-147">Ereditare da `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="31858-147">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="31858-148">La classe `Hub` contiene proprietà ed eventi per gestire connessioni e gruppi, nonché per inviare e ricevere dati.</span><span class="sxs-lookup"><span data-stu-id="31858-148">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="31858-149">Creare il metodo `SendMessage` che invia un messaggio a tutti i client di chat connessi.</span><span class="sxs-lookup"><span data-stu-id="31858-149">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="31858-150">Si noti che il metodo restituisce una classe [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), in quanto SignalR è asincrono.</span><span class="sxs-lookup"><span data-stu-id="31858-150">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="31858-151">Il codice asincrono ha una scalabilità migliore.</span><span class="sxs-lookup"><span data-stu-id="31858-151">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="31858-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="31858-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="31858-153">Aprire la cartella *SignalRChat* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="31858-153">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="31858-154">Aggiungere una classe al progetto selezionando **File** > **Nuovo file** dal menu.</span><span class="sxs-lookup"><span data-stu-id="31858-154">Add a class to the project by selecting **File** > **New File** from the menu.</span></span> <span data-ttu-id="31858-155">Assegnare alla classe il nome `ChatHub` e al file il nome *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="31858-155">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

3. <span data-ttu-id="31858-156">Ereditare da `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="31858-156">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="31858-157">La classe `Hub` contiene proprietà ed eventi per gestire connessioni e gruppi, nonché per inviare e ricevere dati dai client.</span><span class="sxs-lookup"><span data-stu-id="31858-157">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="31858-158">Aggiungere un metodo `SendMessage` alla classe.</span><span class="sxs-lookup"><span data-stu-id="31858-158">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="31858-159">Il metodo `SendMessage` invia un messaggio a tutti i client di chat connessi.</span><span class="sxs-lookup"><span data-stu-id="31858-159">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="31858-160">Si noti che il metodo restituisce una classe [Task](/dotnet/api/system.threading.tasks.task), in quanto SignalR è asincrono.</span><span class="sxs-lookup"><span data-stu-id="31858-160">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="31858-161">Il codice asincrono ha una scalabilità migliore.</span><span class="sxs-lookup"><span data-stu-id="31858-161">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="31858-162">Configurare il progetto per l'uso di SignalR</span><span class="sxs-lookup"><span data-stu-id="31858-162">Configure the project to use SignalR</span></span>

<span data-ttu-id="31858-163">Il server di SignalR deve essere configurato in modo che passi le richieste a SignalR.</span><span class="sxs-lookup"><span data-stu-id="31858-163">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="31858-164">Per configurare un progetto SignalR, modificare il metodo `Startup.ConfigureServices` del progetto.</span><span class="sxs-lookup"><span data-stu-id="31858-164">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="31858-165">`services.AddSignalR` aggiunge SignalR come parte della pipeline del [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="31858-165">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="31858-166">Configurare le route per gli hub usando `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="31858-166">Configure routes to your hubs using `UseSignalR`.</span></span>

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="31858-167">Creare il codice del client SignalR</span><span class="sxs-lookup"><span data-stu-id="31858-167">Create the SignalR client code</span></span>

1. <span data-ttu-id="31858-168">Aggiungere un file JavaScript, denominato *chat.js*, alla cartella *wwwroot\js*.</span><span class="sxs-lookup"><span data-stu-id="31858-168">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="31858-169">Aggiungere il codice seguente al file:</span><span class="sxs-lookup"><span data-stu-id="31858-169">Add the following code to it:</span></span>

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="31858-170">Sostituire il codice in *Pages\Index.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="31858-170">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   <span data-ttu-id="31858-171">Il codice HTML precedente visualizza i campi del nome e del messaggio e un pulsante di invio.</span><span class="sxs-lookup"><span data-stu-id="31858-171">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="31858-172">Si notino i riferimenti dello script nella parte inferiore: un riferimento a SignalR e a *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="31858-172">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="31858-173">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="31858-173">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="31858-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="31858-174">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="31858-175">Selezionare **Debug** > **Avvia senza eseguire debug** per avviare un browser e caricare il sito Web in locale.</span><span class="sxs-lookup"><span data-stu-id="31858-175">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="31858-176">Copiare l'URL dalla barra dell'indirizzo.</span><span class="sxs-lookup"><span data-stu-id="31858-176">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="31858-177">Aprire un'altra istanza di un browser qualsiasi e incollare l'URL nella barra dell'indirizzo.</span><span class="sxs-lookup"><span data-stu-id="31858-177">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="31858-178">Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="31858-178">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="31858-179">Nome e messaggio vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="31858-179">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="31858-180">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="31858-180">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="31858-181">Premere **Debug** (F5) per compilare ed eseguire il programma.</span><span class="sxs-lookup"><span data-stu-id="31858-181">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="31858-182">Quando viene eseguito il programma si apre una finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="31858-182">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="31858-183">Aprire un'altra finestra del browser e caricare il sito Web in locale.</span><span class="sxs-lookup"><span data-stu-id="31858-183">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="31858-184">Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="31858-184">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="31858-185">Nome e messaggio vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="31858-185">The name and message are displayed on both pages instantly.</span></span>

---

  ![Soluzione](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="31858-187">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="31858-187">Related resources</span></span>

[<span data-ttu-id="31858-188">Introduzione a SignalR di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="31858-188">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
