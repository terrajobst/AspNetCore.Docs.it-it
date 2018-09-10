---
title: 'Esercitazione: Introduzione a SignalR in ASP.NET Core'
author: tdykstra
description: In questa esercitazione viene creata un'app di chat che usa SignalR per ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/20/2018
uid: tutorials/signalr
ms.openlocfilehash: a2573e2817a2d8921954264ca17bc3a7e2a010a8
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055832"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="e7c01-103">Esercitazione: Introduzione a SignalR in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7c01-103">Tutorial: Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="e7c01-104">Questa esercitazione illustra le nozioni di base per compilare un'app in tempo reale con SignalR.</span><span class="sxs-lookup"><span data-stu-id="e7c01-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="e7c01-105">Vengono illustrate le seguenti procedure:</span><span class="sxs-lookup"><span data-stu-id="e7c01-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e7c01-106">Creare un'app Web che usa SignalR in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e7c01-106">Create a web app that uses SignalR on ASP.NET Core.</span></span>
> * <span data-ttu-id="e7c01-107">Creare un hub SignalR nel server.</span><span class="sxs-lookup"><span data-stu-id="e7c01-107">Create a SignalR hub on the server.</span></span>
> * <span data-ttu-id="e7c01-108">Connettersi all'hub SignalR da client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e7c01-108">Connect to the SignalR hub from JavaScript clients.</span></span>
> * <span data-ttu-id="e7c01-109">Usare l'hub per inviare messaggi a tutti i client connessi da qualsiasi client.</span><span class="sxs-lookup"><span data-stu-id="e7c01-109">Use the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="e7c01-110">Al termine, si disporrà di un'app di chat funzionante:</span><span class="sxs-lookup"><span data-stu-id="e7c01-110">At the end, you'll have a working chat app:</span></span>

![App SignalR di esempio](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="e7c01-112">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="e7c01-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e7c01-113">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="e7c01-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7c01-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7c01-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e7c01-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) versione 15.7.3 o successive con il carico di lavoro **Sviluppo ASP.NET e Web**</span><span class="sxs-lookup"><span data-stu-id="e7c01-115">[Visual Studio 2017 version 15.7.3 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="e7c01-116">.NET Core SDK 2.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="e7c01-116">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="e7c01-117">[npm](https://www.npmjs.com/get-npm) (gestore pacchetti per Node.js, usato per la libreria client JavaScript di SignalR)</span><span class="sxs-lookup"><span data-stu-id="e7c01-117">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7c01-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7c01-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="e7c01-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7c01-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="e7c01-120">.NET Core SDK 2.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="e7c01-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="e7c01-121">C# per Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7c01-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="e7c01-122">[npm](https://www.npmjs.com/get-npm) (gestore pacchetti per Node.js, usato per la libreria client JavaScript di SignalR)</span><span class="sxs-lookup"><span data-stu-id="e7c01-122">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7c01-123">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e7c01-123">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="e7c01-124">Visual Studio per Mac versione 7.5.4 o successiva</span><span class="sxs-lookup"><span data-stu-id="e7c01-124">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="e7c01-125">[.NET Core SDK 2.1 o versione successiva](https://www.microsoft.com/net/download/all) (incluso nell'installazione di Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="e7c01-125">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>
* <span data-ttu-id="e7c01-126">[npm](https://www.npmjs.com/get-npm) (gestore pacchetti per Node.js, usato per la libreria client JavaScript di SignalR)</span><span class="sxs-lookup"><span data-stu-id="e7c01-126">[npm](https://www.npmjs.com/get-npm) (Package manager for Node.js, used for the SignalR JavaScript client library.)</span></span>

---

## <a name="create-the-project"></a><span data-ttu-id="e7c01-127">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="e7c01-127">Create the project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7c01-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7c01-128">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="e7c01-129">Nel menu selezionare **File > Nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="e7c01-129">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="e7c01-130">Nella finestra di dialogo **Nuovo progetto** selezionare **Installate > Visual C# > Web > Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e7c01-130">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="e7c01-131">Denominare il progetto *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="e7c01-131">Name the project *SignalRChat*.</span></span>

  ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="e7c01-133">Selezionare **Applicazione Web** per creare un progetto che usa Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="e7c01-133">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="e7c01-134">Selezionare un framework di destinazione di **.NET Core**, selezionare **ASP.NET Core 2.1** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="e7c01-134">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7c01-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7c01-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="e7c01-137">Aprire una cartella che è possibile usare per un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="e7c01-137">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="e7c01-138">In **Terminale integrato** eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e7c01-138">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7c01-139">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e7c01-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e7c01-140">Nel menu selezionare **File > Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="e7c01-140">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="e7c01-141">Selezionare **.NET Core > App > App Web ASP.NET Core** (non selezionare **App Web ASP.NET Core (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="e7c01-141">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="e7c01-142">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e7c01-142">Select **Next**.</span></span>

* <span data-ttu-id="e7c01-143">Assegnare al progetto il nome *SignalRChat* e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="e7c01-143">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="e7c01-144">Aggiungere la libreria client di SignalR</span><span class="sxs-lookup"><span data-stu-id="e7c01-144">Add the SignalR client library</span></span>

<span data-ttu-id="e7c01-145">La libreria server di SignalR è inclusa nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="e7c01-145">The SignalR server library is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="e7c01-146">È tuttavia necessario ottenere la libreria client JavaScript da [npm, il sistema di gestione pacchetti di Node.js](https://www.npmjs.com/get-npm).</span><span class="sxs-lookup"><span data-stu-id="e7c01-146">But you have to get the JavaScript client library from [npm, the Node.js package manager](https://www.npmjs.com/get-npm).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7c01-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7c01-147">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="e7c01-148">In **Console di Gestione pacchetti** (PMC), passare alla cartella del progetto, ovvero quella che contiene il file *SignalRChat.csproj*.</span><span class="sxs-lookup"><span data-stu-id="e7c01-148">In **Package Manager Console** (PMC), change to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

  ```console
  cd SignalRChat
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7c01-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7c01-149">Visual Studio Code</span></span>](#tab/visual-studio-code/)

2. <span data-ttu-id="e7c01-150">Passare alla cartella del nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="e7c01-150">Change to the new project folder.</span></span>

  ```console
  cd SignalRChat
  ``` 

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7c01-151">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e7c01-151">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e7c01-152">In **Terminale** passare alla cartella del progetto, ovvero quella che contiene il file *SignalRChat.csproj*.</span><span class="sxs-lookup"><span data-stu-id="e7c01-152">In the **Terminal**, navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

---

* <span data-ttu-id="e7c01-153">Eseguire l'inizializzatore di npm per creare un file *package.json*:</span><span class="sxs-lookup"><span data-stu-id="e7c01-153">Run the npm initializer to create a *package.json* file:</span></span>

  ```console
  npm init -y
  ```

  <span data-ttu-id="e7c01-154">Il comando crea un output simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="e7c01-154">The command creates output similar to the following example:</span></span>

  ```console
  Wrote to C:\tmp\SignalRChat\package.json:
  {
    "name": "SignalRChat",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC"0
  }
  ```

* <span data-ttu-id="e7c01-155">Installare il pacchetto della libreria client:</span><span class="sxs-lookup"><span data-stu-id="e7c01-155">Install the client library package:</span></span>

  ```console
  npm install @aspnet/signalr
  ```

  <span data-ttu-id="e7c01-156">Il comando crea un output simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="e7c01-156">The command creates output similar to the following example:</span></span>

  ```
  npm notice created a lockfile as package-lock.json. You should commit this file.
  npm WARN signalrchat@1.0.0 No description
  npm WARN signalrchat@1.0.0 No repository field.

  + @aspnet/signalr@1.0.2
  added 1 package in 0.98s
  ```

<span data-ttu-id="e7c01-157">Il comando `npm install` ha scaricato la libreria client JavaScript in una sottocartella in *node_modules*.</span><span class="sxs-lookup"><span data-stu-id="e7c01-157">The `npm install` command downloaded the JavaScript client library to a subfolder under *node_modules*.</span></span> <span data-ttu-id="e7c01-158">Copiarla da tale percorso in una cartella in *wwwroot* a cui è possibile fare riferimento dalla pagina Web dell'app di chat.</span><span class="sxs-lookup"><span data-stu-id="e7c01-158">Copy it from there to a folder under *wwwroot* that you can reference from the chat app web page.</span></span>

* <span data-ttu-id="e7c01-159">Creare una cartella *signalr* in *wwwroot/lib*.</span><span class="sxs-lookup"><span data-stu-id="e7c01-159">Create a *signalr* folder in *wwwroot/lib*.</span></span>

* <span data-ttu-id="e7c01-160">Copiare il file *signalr.js* da *node_modules/@aspnet/signalr/dist/browser* alla nuova cartella *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="e7c01-160">Copy the *signalr.js* file from *node_modules/@aspnet/signalr/dist/browser* to the new *wwwroot/lib/signalr* folder.</span></span>

## <a name="create-the-signalr-hub"></a><span data-ttu-id="e7c01-161">Creare l'hub di SignalR</span><span class="sxs-lookup"><span data-stu-id="e7c01-161">Create the SignalR hub</span></span>

<span data-ttu-id="e7c01-162">Un [hub](xref:signalr/hubs) è una classe usata come pipeline di alto livello che gestisce le comunicazioni client-server.</span><span class="sxs-lookup"><span data-stu-id="e7c01-162">A [hub](xref:signalr/hubs) is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="e7c01-163">Nella cartella del progetto SignalRChat creare una cartella *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="e7c01-163">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="e7c01-164">Nella cartella *Hubs* creare un file *ChatHub.cs* contenente il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="e7c01-164">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="e7c01-165">La classe `ChatHub` eredita dalla classe [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) di SignalR.</span><span class="sxs-lookup"><span data-stu-id="e7c01-165">The `ChatHub` class inherits from the SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) class.</span></span> <span data-ttu-id="e7c01-166">La classe `Hub` gestisce connessioni, gruppi e messaggistica.</span><span class="sxs-lookup"><span data-stu-id="e7c01-166">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="e7c01-167">Il metodo `SendMessage`, che può essere chiamato da qualsiasi client connesso,</span><span class="sxs-lookup"><span data-stu-id="e7c01-167">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="e7c01-168">invia il messaggio ricevuto a tutti i client.</span><span class="sxs-lookup"><span data-stu-id="e7c01-168">It sends the received message to all clients.</span></span> <span data-ttu-id="e7c01-169">Il codice di SignalR è asincrono allo scopo di offrire la massima scalabilità.</span><span class="sxs-lookup"><span data-stu-id="e7c01-169">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="e7c01-170">Configurare il progetto per l'uso di SignalR</span><span class="sxs-lookup"><span data-stu-id="e7c01-170">Configure the project to use SignalR</span></span>

<span data-ttu-id="e7c01-171">È necessario configurare il server SignalR in modo che passi le richieste SignalR a SignalR.</span><span class="sxs-lookup"><span data-stu-id="e7c01-171">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="e7c01-172">Aggiungere il codice evidenziato seguente al file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="e7c01-172">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="e7c01-173">Con queste modifiche SignalR viene aggiunto al sistema di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) e alla pipeline [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="e7c01-173">These changes add SignalR to the [dependency injection](xref:fundamentals/dependency-injection) system and the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="e7c01-174">Creare il codice del client SignalR</span><span class="sxs-lookup"><span data-stu-id="e7c01-174">Create the SignalR client code</span></span>

* <span data-ttu-id="e7c01-175">Sostituire il codice in *Pages\Index.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="e7c01-175">Replace the content in *Pages\Index.cshtml* with the following:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="e7c01-176">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="e7c01-176">The preceding code:</span></span>

  * <span data-ttu-id="e7c01-177">Crea le caselle di testo per il nome e il messaggio di testo, nonché un pulsante di invio.</span><span class="sxs-lookup"><span data-stu-id="e7c01-177">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="e7c01-178">Crea un elenco con `id="messagesList"` per visualizzare i messaggi ricevuti dall'hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="e7c01-178">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="e7c01-179">Include i riferimenti agli script in SignalR e il codice dell'applicazione *chat.js* creato nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="e7c01-179">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="e7c01-180">Nella cartella *wwwroot/js* creare un file *chat.js* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="e7c01-180">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="e7c01-181">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="e7c01-181">The preceding code:</span></span>

  * <span data-ttu-id="e7c01-182">Crea e avvia una connessione.</span><span class="sxs-lookup"><span data-stu-id="e7c01-182">Creates and starts a connection.</span></span>
  * <span data-ttu-id="e7c01-183">Aggiunge al pulsante di invio un gestore che invia messaggi all'hub.</span><span class="sxs-lookup"><span data-stu-id="e7c01-183">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="e7c01-184">Aggiunge all'oggetto connessione un gestore che riceve i messaggi dall'hub e li aggiunge all'elenco.</span><span class="sxs-lookup"><span data-stu-id="e7c01-184">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="e7c01-185">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="e7c01-185">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e7c01-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e7c01-186">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e7c01-187">Premere **CTRL+F5** per eseguire l'app senza debug.</span><span class="sxs-lookup"><span data-stu-id="e7c01-187">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e7c01-188">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e7c01-188">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e7c01-189">Premere **CTRL+F5** per eseguire l'app senza debug.</span><span class="sxs-lookup"><span data-stu-id="e7c01-189">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e7c01-190">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e7c01-190">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e7c01-191">Nel menu selezionare **Esegui > Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="e7c01-191">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="e7c01-192">Copiare l'URL dalla barra degli indirizzi, aprire un'altra istanza o scheda del browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="e7c01-192">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="e7c01-193">Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="e7c01-193">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="e7c01-194">Nome e messaggio vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="e7c01-194">The name and message are displayed on both pages instantly.</span></span>

  ![App SignalR di esempio](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="e7c01-196">Se l'app non funziona, aprire gli strumenti di sviluppo (F12) del browser e passare alla console.</span><span class="sxs-lookup"><span data-stu-id="e7c01-196">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="e7c01-197">È possibile che vengano visualizzati errori correlati al codice HTML e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e7c01-197">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="e7c01-198">Ad esempio, si supponga di aver inserito *signalr.js* in una cartella diversa da quella indicata nelle istruzioni.</span><span class="sxs-lookup"><span data-stu-id="e7c01-198">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="e7c01-199">In questo caso il riferimento a tale file non funzionerà e verrà visualizzato un errore 404 nella console.</span><span class="sxs-lookup"><span data-stu-id="e7c01-199">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="e7c01-200">![Errore di signalr.js non trovato](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="e7c01-200">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="e7c01-201">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="e7c01-201">Next steps</span></span>

<span data-ttu-id="e7c01-202">Se si vuole che i client si connettano a un'app SignalR da domini diversi, è necessario abilitare Condivisione risorse tra le origini (CORS).</span><span class="sxs-lookup"><span data-stu-id="e7c01-202">If you want clients to connect to a SignalR app from different domains, you have to enable Cross-Origin Resource Sharing (CORS).</span></span> <span data-ttu-id="e7c01-203">Per altre informazioni, vedere [Condivisione risorse tra le origini](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span><span class="sxs-lookup"><span data-stu-id="e7c01-203">For more information, see [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span></span>

<span data-ttu-id="e7c01-204">Per altre informazioni su SignalR, hub e client JavaScript, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="e7c01-204">To learn more about SignalR, hubs, and JavaScript clients, see these resources:</span></span>

* [<span data-ttu-id="e7c01-205">Introduzione a SignalR per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7c01-205">Introduction to SignalR for ASP.NET Core</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="e7c01-206">Usare gli hub in SignalR per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7c01-206">Use hubs in SignalR for ASP.NET Core</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="e7c01-207">Client JavaScript di SignalR per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7c01-207">ASP.NET Core SignalR JavaScript client</span></span>](xref:signalr/javascript-client)
