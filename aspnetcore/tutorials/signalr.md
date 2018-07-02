---
title: Introduzione a SignalR in ASP.NET Core
author: rachelappel
description: In questa esercitazione viene creata un'app usando SignalR per ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 8762a4be1032d58014dd32dfdd3707197e14c6f9
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36297200"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="51202-103">Introduzione a SignalR in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="51202-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="51202-104">[Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="51202-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="51202-105">Questa esercitazione illustra le nozioni di base per compilare un'app in tempo reale con SignalR per ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="51202-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Soluzione](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="51202-107">In questa esercitazione vengono illustrate le attività di sviluppo di SignalR seguenti:</span><span class="sxs-lookup"><span data-stu-id="51202-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="51202-108">Creare un servizio SignalR in un'app Web di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="51202-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="51202-109">Creare un hub SignalR per eseguire il push dei contenuti ai client.</span><span class="sxs-lookup"><span data-stu-id="51202-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="51202-110">Modificare la classe `Startup` e configurare l'app.</span><span class="sxs-lookup"><span data-stu-id="51202-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="51202-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="51202-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="51202-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="51202-112">Prerequisites</span></span>

<span data-ttu-id="51202-113">Installare il software seguente:</span><span class="sxs-lookup"><span data-stu-id="51202-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="51202-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="51202-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="51202-115">.NET Core SDK 2.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="51202-115">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="51202-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) versione 15.7 o successiva con il carico di lavoro **Sviluppo ASP.NET e Web**</span><span class="sxs-lookup"><span data-stu-id="51202-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="51202-117">npm</span><span class="sxs-lookup"><span data-stu-id="51202-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="51202-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="51202-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="51202-119">.NET Core SDK 2.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="51202-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="51202-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="51202-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="51202-121">C# per Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="51202-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="51202-122">npm</span><span class="sxs-lookup"><span data-stu-id="51202-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="51202-123">Creare un progetto ASP.NET Core che ospita il client e il server SignalR</span><span class="sxs-lookup"><span data-stu-id="51202-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="51202-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="51202-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="51202-125">Selezionare l'opzione di menu **File** > **Nuovo progetto** e scegliere **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="51202-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="51202-126">Denominare il progetto *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="51202-126">Name the project *SignalRChat*.</span></span>

   ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="51202-128">Selezionare **Applicazione Web** per creare un progetto usando le Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="51202-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="51202-129">Selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="51202-129">Then select **OK**.</span></span> <span data-ttu-id="51202-130">Assicurarsi che nel selettore del framework sia selezionato **ASP.NET Core 2.1**, anche se SignalR viene eseguito in versioni precedenti di .NET.</span><span class="sxs-lookup"><span data-stu-id="51202-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="51202-132">Visual Studio include il pacchetto `Microsoft.AspNetCore.SignalR` contenente le relative librerie server come parte del modello **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="51202-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="51202-133">Tuttavia, la libreria client JavaScript per SignalR deve essere installata usando *npm*.</span><span class="sxs-lookup"><span data-stu-id="51202-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="51202-134">Eseguire i comandi seguenti nella finestra **Console di gestione pacchetti**, dalla radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="51202-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. <span data-ttu-id="51202-135">Creare una nuova cartella denominata "signalr" all'interno della cartella *lib* nel progetto.</span><span class="sxs-lookup"><span data-stu-id="51202-135">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="51202-136">Copiare il file *signalr.js* da *node_modules\\@aspnet\signalr\dist\browser* in questa cartella.</span><span class="sxs-lookup"><span data-stu-id="51202-136">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="51202-137">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="51202-137">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="51202-138">Da **Terminale integrato** eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="51202-138">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="51202-139">Installare la libreria client JavaScript usando *npm*.</span><span class="sxs-lookup"><span data-stu-id="51202-139">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="51202-140">Creare una nuova cartella denominata "signalr" all'interno della cartella *lib* nel progetto.</span><span class="sxs-lookup"><span data-stu-id="51202-140">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="51202-141">Copiare il file *signalr.js* da *node_modules\\@aspnet\signalr\dist\browser* in questa cartella.</span><span class="sxs-lookup"><span data-stu-id="51202-141">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="51202-142">Creare l'hub di SignalR</span><span class="sxs-lookup"><span data-stu-id="51202-142">Create the SignalR Hub</span></span>

<span data-ttu-id="51202-143">Un hub è una classe che funge da pipeline di alto livello che consente al client e al server di chiamare metodi tra loro.</span><span class="sxs-lookup"><span data-stu-id="51202-143">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="51202-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="51202-144">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="51202-145">Aggiungere una classe al progetto scegliendo **File** > **Nuovo** > **File** e selezionando **Classe di Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="51202-145">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="51202-146">Denominare il file *ChatHub*.</span><span class="sxs-lookup"><span data-stu-id="51202-146">Name the file *ChatHub*.</span></span>

2. <span data-ttu-id="51202-147">Ereditare da `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="51202-147">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="51202-148">La classe `Hub` contiene proprietà ed eventi per gestire connessioni e gruppi, nonché per inviare e ricevere dati.</span><span class="sxs-lookup"><span data-stu-id="51202-148">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="51202-149">Creare il metodo `SendMessage` che invia un messaggio a tutti i client di chat connessi.</span><span class="sxs-lookup"><span data-stu-id="51202-149">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="51202-150">Si noti che il metodo restituisce una classe [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), in quanto SignalR è asincrono.</span><span class="sxs-lookup"><span data-stu-id="51202-150">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="51202-151">Il codice asincrono ha una scalabilità migliore.</span><span class="sxs-lookup"><span data-stu-id="51202-151">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="51202-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="51202-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="51202-153">Aprire la cartella *SignalRChat* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="51202-153">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="51202-154">Aggiungere una classe al progetto selezionando **File** > **Nuovo file** dal menu.</span><span class="sxs-lookup"><span data-stu-id="51202-154">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

3. <span data-ttu-id="51202-155">Ereditare da `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="51202-155">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="51202-156">La classe `Hub` contiene proprietà ed eventi per gestire connessioni e gruppi, nonché per inviare e ricevere dati dai client.</span><span class="sxs-lookup"><span data-stu-id="51202-156">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="51202-157">Aggiungere un metodo `SendMessage` alla classe.</span><span class="sxs-lookup"><span data-stu-id="51202-157">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="51202-158">Il metodo `SendMessage` invia un messaggio a tutti i client di chat connessi.</span><span class="sxs-lookup"><span data-stu-id="51202-158">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="51202-159">Si noti che il metodo restituisce una classe [Task](/dotnet/api/system.threading.tasks.task), in quanto SignalR è asincrono.</span><span class="sxs-lookup"><span data-stu-id="51202-159">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="51202-160">Il codice asincrono ha una scalabilità migliore.</span><span class="sxs-lookup"><span data-stu-id="51202-160">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs?range=6-12)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="51202-161">Configurare il progetto per l'uso di SignalR</span><span class="sxs-lookup"><span data-stu-id="51202-161">Configure the project to use SignalR</span></span>

<span data-ttu-id="51202-162">Il server di SignalR deve essere configurato in modo che passi le richieste a SignalR.</span><span class="sxs-lookup"><span data-stu-id="51202-162">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="51202-163">Per configurare un progetto SignalR, modificare il metodo `Startup.ConfigureServices` del progetto.</span><span class="sxs-lookup"><span data-stu-id="51202-163">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="51202-164">`services.AddSignalR` aggiunge SignalR come parte della pipeline del [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="51202-164">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

2. <span data-ttu-id="51202-165">Configurare le route per gli hub usando `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="51202-165">Configure routes to your hubs using `UseSignalR`.</span></span>

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="51202-166">Creare il codice del client SignalR</span><span class="sxs-lookup"><span data-stu-id="51202-166">Create the SignalR client code</span></span>

1. <span data-ttu-id="51202-167">Aggiungere un file JavaScript, denominato *chat.js*, alla cartella *wwwroot\js*.</span><span class="sxs-lookup"><span data-stu-id="51202-167">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="51202-168">Aggiungere il codice seguente al file:</span><span class="sxs-lookup"><span data-stu-id="51202-168">Add the following code to it:</span></span>

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="51202-169">Sostituire il codice in *Pages\Index.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="51202-169">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   <span data-ttu-id="51202-170">Il codice HTML precedente visualizza i campi del nome e del messaggio e un pulsante di invio.</span><span class="sxs-lookup"><span data-stu-id="51202-170">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="51202-171">Si notino i riferimenti dello script nella parte inferiore: un riferimento a SignalR e a *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="51202-171">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="51202-172">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="51202-172">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="51202-173">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="51202-173">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="51202-174">Selezionare **Debug** > **Avvia senza eseguire debug** per avviare un browser e caricare il sito Web in locale.</span><span class="sxs-lookup"><span data-stu-id="51202-174">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="51202-175">Copiare l'URL dalla barra dell'indirizzo.</span><span class="sxs-lookup"><span data-stu-id="51202-175">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="51202-176">Aprire un'altra istanza di un browser qualsiasi e incollare l'URL nella barra dell'indirizzo.</span><span class="sxs-lookup"><span data-stu-id="51202-176">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="51202-177">Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="51202-177">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="51202-178">Nome e messaggio vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="51202-178">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="51202-179">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="51202-179">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="51202-180">Premere **Debug** (F5) per compilare ed eseguire il programma.</span><span class="sxs-lookup"><span data-stu-id="51202-180">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="51202-181">Quando viene eseguito il programma si apre una finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="51202-181">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="51202-182">Aprire un'altra finestra del browser e caricare il sito Web in locale.</span><span class="sxs-lookup"><span data-stu-id="51202-182">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="51202-183">Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="51202-183">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="51202-184">Nome e messaggio vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="51202-184">The name and message are displayed on both pages instantly.</span></span>

---

  ![Soluzione](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="51202-186">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="51202-186">Related resources</span></span>

[<span data-ttu-id="51202-187">Introduzione a SignalR di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="51202-187">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
