---
title: Introduzione a SignalR in ASP.NET Core
author: tdykstra
description: In questa esercitazione viene creata un'app usando SignalR per ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 83be28b30cf06eeea37e8d76b3f6444ffd9a10e8
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095491"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="c6cb1-103">Introduzione a SignalR in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c6cb1-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="c6cb1-104">Questa esercitazione illustra le nozioni di base per compilare un'app in tempo reale con SignalR per ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-104">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Soluzione](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="c6cb1-106">In questa esercitazione vengono illustrate le attività di sviluppo di SignalR seguenti:</span><span class="sxs-lookup"><span data-stu-id="c6cb1-106">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c6cb1-107">Creare un servizio SignalR in un'app Web di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-107">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="c6cb1-108">Creare un hub SignalR per eseguire il push dei contenuti ai client.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-108">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="c6cb1-109">Modificare la classe `Startup` e configurare l'app.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-109">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="c6cb1-110">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c6cb1-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c6cb1-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="c6cb1-111">Prerequisites</span></span>

<span data-ttu-id="c6cb1-112">Installare il software seguente:</span><span class="sxs-lookup"><span data-stu-id="c6cb1-112">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c6cb1-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c6cb1-113">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="c6cb1-114">.NET Core SDK 2.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="c6cb1-114">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="c6cb1-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) versione 15.7.3 o successive con il carico di lavoro **Sviluppo ASP.NET e Web**</span><span class="sxs-lookup"><span data-stu-id="c6cb1-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the **ASP.NET and web development** workload</span></span>
* <span data-ttu-id="c6cb1-116">[npm](https://www.npmjs.com/get-npm) (Gestione pacchetti per Node.js)</span><span class="sxs-lookup"><span data-stu-id="c6cb1-116">[npm](https://www.npmjs.com/get-npm) (package manager for Node.js)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c6cb1-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c6cb1-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="c6cb1-118">.NET Core SDK 2.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="c6cb1-118">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="c6cb1-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c6cb1-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="c6cb1-120">C# per Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c6cb1-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="c6cb1-121">[npm](https://www.npmjs.com/get-npm) (Gestione pacchetti per Node.js)</span><span class="sxs-lookup"><span data-stu-id="c6cb1-121">[npm](https://www.npmjs.com/get-npm) (package manager for Node.js)</span></span>

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="c6cb1-122">Creare un progetto ASP.NET Core che ospita il client e il server SignalR</span><span class="sxs-lookup"><span data-stu-id="c6cb1-122">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c6cb1-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c6cb1-123">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="c6cb1-124">Selezionare l'opzione di menu **File** > **Nuovo progetto** e scegliere **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-124">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="c6cb1-125">Denominare il progetto *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-125">Name the project *SignalRChat*.</span></span>

   ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="c6cb1-127">Selezionare **Applicazione Web** per creare un progetto usando le Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-127">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="c6cb1-128">Selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-128">Then select **OK**.</span></span> <span data-ttu-id="c6cb1-129">Assicurarsi che nel selettore del framework sia selezionato **ASP.NET Core 2.1**, anche se SignalR viene eseguito in versioni precedenti di .NET.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-129">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="c6cb1-131">Visual Studio include il pacchetto `Microsoft.AspNetCore.SignalR` contenente le relative librerie server come parte del modello **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-131">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="c6cb1-132">Tuttavia, la libreria client JavaScript per SignalR deve essere installata usando *npm*.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-132">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="c6cb1-133">Eseguire i comandi seguenti nella finestra **Console di gestione pacchetti**, dalla radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="c6cb1-133">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. <span data-ttu-id="c6cb1-134">Creare una nuova cartella denominata "signalr" all'interno della cartella *wwwroot/lib* nel progetto.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-134">Create a new folder named "signalr" inside the  *wwwroot/lib* folder in your project.</span></span> <span data-ttu-id="c6cb1-135">Copiare il file *signalr.js* da *node_modules\\@aspnet\signalr\dist\browser* in questa cartella.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c6cb1-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c6cb1-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="c6cb1-137">Da **Terminale integrato** eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c6cb1-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="c6cb1-138">Installare la libreria client JavaScript usando *npm*.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-138">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="c6cb1-139">Creare una nuova cartella denominata "signalr" all'interno della cartella *lib* nel progetto.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-139">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="c6cb1-140">Copiare il file *signalr.js* da *node_modules\\@aspnet\signalr\dist\browser* in questa cartella.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-140">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="c6cb1-141">Creare l'hub di SignalR</span><span class="sxs-lookup"><span data-stu-id="c6cb1-141">Create the SignalR Hub</span></span>

<span data-ttu-id="c6cb1-142">Un hub è una classe che funge da pipeline di alto livello che consente al client e al server di chiamare metodi tra loro.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-142">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c6cb1-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c6cb1-143">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="c6cb1-144">Aggiungere una classe al progetto scegliendo **File** > **Nuovo** > **File** e selezionando **Classe di Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-144">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="c6cb1-145">Assegnare alla classe il nome `ChatHub` e al file il nome *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-145">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

2. <span data-ttu-id="c6cb1-146">Ereditare da `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-146">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="c6cb1-147">La classe `Hub` contiene proprietà ed eventi per gestire connessioni e gruppi, nonché per inviare e ricevere dati.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-147">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="c6cb1-148">Creare il metodo `SendMessage` che invia un messaggio a tutti i client di chat connessi.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-148">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="c6cb1-149">Si noti che il metodo restituisce una classe [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), in quanto SignalR è asincrono.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-149">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="c6cb1-150">Il codice asincrono ha una scalabilità migliore.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-150">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c6cb1-151">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c6cb1-151">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="c6cb1-152">Aprire la cartella *SignalRChat* in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-152">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="c6cb1-153">Aggiungere una classe al progetto selezionando **File** > **Nuovo file** dal menu.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-153">Add a class to the project by selecting **File** > **New File** from the menu.</span></span> <span data-ttu-id="c6cb1-154">Assegnare alla classe il nome `ChatHub` e al file il nome *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-154">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

3. <span data-ttu-id="c6cb1-155">Ereditare da `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-155">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="c6cb1-156">La classe `Hub` contiene proprietà ed eventi per gestire connessioni e gruppi, nonché per inviare e ricevere dati dai client.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-156">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="c6cb1-157">Aggiungere un metodo `SendMessage` alla classe.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-157">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="c6cb1-158">Il metodo `SendMessage` invia un messaggio a tutti i client di chat connessi.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-158">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="c6cb1-159">Si noti che il metodo restituisce una classe [Task](/dotnet/api/system.threading.tasks.task), in quanto SignalR è asincrono.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-159">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="c6cb1-160">Il codice asincrono ha una scalabilità migliore.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-160">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="c6cb1-161">Configurare il progetto per l'uso di SignalR</span><span class="sxs-lookup"><span data-stu-id="c6cb1-161">Configure the project to use SignalR</span></span>

<span data-ttu-id="c6cb1-162">Il server di SignalR deve essere configurato in modo che passi le richieste a SignalR.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-162">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="c6cb1-163">Per configurare un progetto SignalR, modificare il metodo `Startup.ConfigureServices` del progetto.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-163">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="c6cb1-164">`services.AddSignalR` rende disponibili i servizi SignalR per il sistema di [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c6cb1-164">`services.AddSignalR` makes the SignalR services available to the [dependency injection](xref:fundamentals/dependency-injection) system.</span></span>

1. <span data-ttu-id="c6cb1-165">Configurare le route per gli hub con `UseSignalR` nel metodo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-165">Configure routes to your hubs with `UseSignalR` in the `Configure` method.</span></span> <span data-ttu-id="c6cb1-166">`app.UseSignalR` aggiunge SignalR alla pipeline del [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="c6cb1-166">`app.UseSignalR` adds SignalR to the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="c6cb1-167">Creare il codice del client SignalR</span><span class="sxs-lookup"><span data-stu-id="c6cb1-167">Create the SignalR client code</span></span>

1. <span data-ttu-id="c6cb1-168">Aggiungere un file JavaScript, denominato *chat.js*, alla cartella *wwwroot\js*.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-168">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="c6cb1-169">Aggiungere il codice seguente al file:</span><span class="sxs-lookup"><span data-stu-id="c6cb1-169">Add the following code to it:</span></span>

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="c6cb1-170">Sostituire il codice in *Pages\Index.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c6cb1-170">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   <span data-ttu-id="c6cb1-171">Il codice HTML precedente visualizza i campi del nome e del messaggio e un pulsante di invio.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-171">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="c6cb1-172">Si notino i riferimenti dello script nella parte inferiore: un riferimento a SignalR e a *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-172">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="c6cb1-173">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="c6cb1-173">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c6cb1-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c6cb1-174">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="c6cb1-175">Selezionare **Debug** > **Avvia senza eseguire debug** per avviare un browser e caricare il sito Web in locale.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-175">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="c6cb1-176">Copiare l'URL dalla barra dell'indirizzo.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-176">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="c6cb1-177">Aprire un'altra istanza di un browser qualsiasi e incollare l'URL nella barra dell'indirizzo.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-177">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="c6cb1-178">Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-178">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="c6cb1-179">Nome e messaggio vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-179">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c6cb1-180">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c6cb1-180">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="c6cb1-181">Premere **Debug** (F5) per compilare ed eseguire il programma.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-181">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="c6cb1-182">Quando viene eseguito il programma si apre una finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-182">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="c6cb1-183">Aprire un'altra finestra del browser e caricare il sito Web in locale.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-183">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="c6cb1-184">Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-184">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="c6cb1-185">Nome e messaggio vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="c6cb1-185">The name and message are displayed on both pages instantly.</span></span>

---

  ![Soluzione](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="c6cb1-187">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="c6cb1-187">Related resources</span></span>

[<span data-ttu-id="c6cb1-188">Introduzione a SignalR di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c6cb1-188">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
