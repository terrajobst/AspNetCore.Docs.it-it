---
title: 'Esercitazione: Introduzione a SignalR in ASP.NET Core'
author: tdykstra
description: In questa esercitazione viene creata un'app di chat che usa SignalR per ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: 8581628b3f0cf878b8cc1d0684046d22a8374af9
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523220"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="c648c-103">Esercitazione: Introduzione a SignalR in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c648c-103">Tutorial: Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="c648c-104">Questa esercitazione illustra le nozioni di base per compilare un'app in tempo reale con SignalR.</span><span class="sxs-lookup"><span data-stu-id="c648c-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="c648c-105">Vengono illustrate le seguenti procedure:</span><span class="sxs-lookup"><span data-stu-id="c648c-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c648c-106">Creare un'app Web che usa SignalR in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c648c-106">Create a web app that uses SignalR on ASP.NET Core.</span></span>
> * <span data-ttu-id="c648c-107">Creare un hub SignalR nel server.</span><span class="sxs-lookup"><span data-stu-id="c648c-107">Create a SignalR hub on the server.</span></span>
> * <span data-ttu-id="c648c-108">Connettersi all'hub SignalR da client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c648c-108">Connect to the SignalR hub from JavaScript clients.</span></span>
> * <span data-ttu-id="c648c-109">Usare l'hub per inviare messaggi a tutti i client connessi da qualsiasi client.</span><span class="sxs-lookup"><span data-stu-id="c648c-109">Use the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="c648c-110">Al termine, si disporrà di un'app di chat funzionante:</span><span class="sxs-lookup"><span data-stu-id="c648c-110">At the end, you'll have a working chat app:</span></span>

![App SignalR di esempio](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="c648c-112">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="c648c-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c648c-113">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="c648c-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c648c-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c648c-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c648c-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) versione 15.8 o successive con il carico di lavoro **Sviluppo ASP.NET e Web**</span><span class="sxs-lookup"><span data-stu-id="c648c-115">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="c648c-116">.NET Core SDK 2.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="c648c-116">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c648c-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c648c-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="c648c-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c648c-118">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="c648c-119">.NET Core SDK 2.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="c648c-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="c648c-120">C# per Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c648c-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c648c-121">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="c648c-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="c648c-122">Visual Studio per Mac versione 7.5.4 o successiva</span><span class="sxs-lookup"><span data-stu-id="c648c-122">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="c648c-123">[.NET Core SDK 2.1 o versione successiva](https://www.microsoft.com/net/download/all) (incluso nell'installazione di Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="c648c-123">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-the-project"></a><span data-ttu-id="c648c-124">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="c648c-124">Create the project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c648c-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c648c-125">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="c648c-126">Nel menu selezionare **File > Nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="c648c-126">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="c648c-127">Nella finestra di dialogo **Nuovo progetto** selezionare **Installate > Visual C# > Web > Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c648c-127">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="c648c-128">Denominare il progetto *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="c648c-128">Name the project *SignalRChat*.</span></span>

  ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="c648c-130">Selezionare **Applicazione Web** per creare un progetto che usa Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c648c-130">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="c648c-131">Selezionare un framework di destinazione di **.NET Core**, selezionare **ASP.NET Core 2.1** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="c648c-131">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c648c-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c648c-133">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="c648c-134">Aprire una cartella che è possibile usare per un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="c648c-134">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="c648c-135">In **Terminale integrato** eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c648c-135">In the **Integrated Terminal**, run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c648c-136">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="c648c-136">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c648c-137">Nel menu selezionare **File > Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="c648c-137">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="c648c-138">Selezionare **.NET Core > App > App Web ASP.NET Core** (non selezionare **App Web ASP.NET Core (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="c648c-138">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="c648c-139">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="c648c-139">Select **Next**.</span></span>

* <span data-ttu-id="c648c-140">Assegnare al progetto il nome *SignalRChat* e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="c648c-140">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="c648c-141">Aggiungere la libreria client di SignalR</span><span class="sxs-lookup"><span data-stu-id="c648c-141">Add the SignalR client library</span></span>

<span data-ttu-id="c648c-142">La libreria server di SignalR è inclusa nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="c648c-142">The SignalR server library is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="c648c-143">La libreria client JavaScript non viene inclusa automaticamente nel progetto.</span><span class="sxs-lookup"><span data-stu-id="c648c-143">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="c648c-144">Per questa esercitazione, usare [Gestione librerie (LibMan)](xref:client-side/libman/index) per ottenere la libreria client da *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="c648c-144">For this tutorial, you use [Library Manager (LibMan)](xref:client-side/libman/index) to get the client library from *unpkg*.</span></span> <span data-ttu-id="c648c-145">[unpkg](https://unpkg.com/#/) è una [rete per la distribuzione di contenuti](https://wikipedia.org/wiki/Content_delivery_network) che può offrire qualsiasi contenuto disponibile in [npm, la gestione pacchetti di Node.js](https://www.npmjs.com/get-npm).</span><span class="sxs-lookup"><span data-stu-id="c648c-145">[unpkg](https://unpkg.com/#/) is a [content delivery network](https://wikipedia.org/wiki/Content_delivery_network) that can deliver anything found in [npm, the Node.js package manager](https://www.npmjs.com/get-npm).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c648c-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c648c-146">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="c648c-147">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **Client-Side Library** (Libreria lato client).</span><span class="sxs-lookup"><span data-stu-id="c648c-147">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="c648c-148">Nella finestra di dialogo **Add Client-Side Library** (Aggiungi libreria lato client) selezionare **unpkg** in **Provider**.</span><span class="sxs-lookup"><span data-stu-id="c648c-148">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="c648c-149">In **Libreria** immettere _@aspnet/signalr@1_e selezionare la versione più recente che non sia di anteprima.</span><span class="sxs-lookup"><span data-stu-id="c648c-149">For **Library**, enter _@aspnet/signalr@1_, and select the latest version that isn't preview.</span></span>

  ![Finestra di dialogo Add Client-Side Library (Aggiungi libreria lato client) - selezione della libreria](signalr/_static/libman1.png)

* <span data-ttu-id="c648c-151">Selezionare **Choose specific files** (Scegli file specifici), espandere la cartella *dist/browser* e selezionare *signalr.js* e *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="c648c-151">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="c648c-152">Impostare **Percorso di destinazione** su *wwwroot/lib/signalr/* e selezionare **Installa**.</span><span class="sxs-lookup"><span data-stu-id="c648c-152">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Finestra di dialogo Add Client-Side Library (Aggiungi libreria lato client) - selezione di file e destinazione](signalr/_static/libman2.png)

  <span data-ttu-id="c648c-154">[LibMan](xref:client-side/libman/index) crea una cartella *wwwroot/lib/signalr* e copia i file selezionati in tale cartella.</span><span class="sxs-lookup"><span data-stu-id="c648c-154">[LibMan](xref:client-side/libman/index) creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c648c-155">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c648c-155">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="c648c-156">In **Terminale integrato** eseguire il comando seguente per installare LibMan.</span><span class="sxs-lookup"><span data-stu-id="c648c-156">In the **Integrated Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="c648c-157">Passare alla cartella del progetto, ovvero quella che contiene il file *SignalRChat.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c648c-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="c648c-158">Eseguire il comando seguente per ottenere la libreria client di SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="c648c-158">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="c648c-159">Potrebbe essere necessario attendere alcuni secondi prima di visualizzare l'output.</span><span class="sxs-lookup"><span data-stu-id="c648c-159">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot\lib\signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="c648c-160">I parametri specificano le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="c648c-160">The parameters specify the following options:</span></span>
  * <span data-ttu-id="c648c-161">Usare il provider unpkg.</span><span class="sxs-lookup"><span data-stu-id="c648c-161">Use the unpkg provider.</span></span>
  * <span data-ttu-id="c648c-162">Copiare i file nella destinazione *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="c648c-162">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="c648c-163">Copiare solo i file specificati.</span><span class="sxs-lookup"><span data-stu-id="c648c-163">Copy only the specified files.</span></span>

  <span data-ttu-id="c648c-164">L'output è simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="c648c-164">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot\lib\signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c648c-165">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="c648c-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c648c-166">Nel **Terminale** eseguire il comando seguente per installare LibMan.</span><span class="sxs-lookup"><span data-stu-id="c648c-166">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="c648c-167">Passare alla cartella del progetto, ovvero quella che contiene il file *SignalRChat.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c648c-167">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="c648c-168">Eseguire il comando seguente per ottenere la libreria client di SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="c648c-168">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot\lib\signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="c648c-169">I parametri specificano le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="c648c-169">The parameters specify the following options:</span></span>
  * <span data-ttu-id="c648c-170">Usare il provider unpkg.</span><span class="sxs-lookup"><span data-stu-id="c648c-170">Use the unpkg provider.</span></span>
  * <span data-ttu-id="c648c-171">Copiare i file nella destinazione *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="c648c-171">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="c648c-172">Copiare solo i file specificati.</span><span class="sxs-lookup"><span data-stu-id="c648c-172">Copy only the specified files.</span></span>

  <span data-ttu-id="c648c-173">L'output è simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="c648c-173">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot\lib\signalr"
  ```

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="c648c-174">Creare l'hub di SignalR</span><span class="sxs-lookup"><span data-stu-id="c648c-174">Create the SignalR hub</span></span>

<span data-ttu-id="c648c-175">Un [hub](xref:signalr/hubs) è una classe usata come pipeline di alto livello che gestisce le comunicazioni client-server.</span><span class="sxs-lookup"><span data-stu-id="c648c-175">A [hub](xref:signalr/hubs) is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="c648c-176">Nella cartella del progetto SignalRChat creare una cartella *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="c648c-176">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="c648c-177">Nella cartella *Hubs* creare un file *ChatHub.cs* contenente il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c648c-177">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="c648c-178">La classe `ChatHub` eredita dalla classe [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) di SignalR.</span><span class="sxs-lookup"><span data-stu-id="c648c-178">The `ChatHub` class inherits from the SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) class.</span></span> <span data-ttu-id="c648c-179">La classe `Hub` gestisce connessioni, gruppi e messaggistica.</span><span class="sxs-lookup"><span data-stu-id="c648c-179">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="c648c-180">Il metodo `SendMessage`, che può essere chiamato da qualsiasi client connesso,</span><span class="sxs-lookup"><span data-stu-id="c648c-180">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="c648c-181">invia il messaggio ricevuto a tutti i client.</span><span class="sxs-lookup"><span data-stu-id="c648c-181">It sends the received message to all clients.</span></span> <span data-ttu-id="c648c-182">Il codice di SignalR è asincrono allo scopo di offrire la massima scalabilità.</span><span class="sxs-lookup"><span data-stu-id="c648c-182">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="c648c-183">Configurare il progetto per l'uso di SignalR</span><span class="sxs-lookup"><span data-stu-id="c648c-183">Configure the project to use SignalR</span></span>

<span data-ttu-id="c648c-184">È necessario configurare il server SignalR in modo che passi le richieste SignalR a SignalR.</span><span class="sxs-lookup"><span data-stu-id="c648c-184">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="c648c-185">Aggiungere il codice evidenziato seguente al file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="c648c-185">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="c648c-186">Con queste modifiche SignalR viene aggiunto al sistema di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) e alla pipeline [middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="c648c-186">These changes add SignalR to the [dependency injection](xref:fundamentals/dependency-injection) system and the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="c648c-187">Creare il codice del client SignalR</span><span class="sxs-lookup"><span data-stu-id="c648c-187">Create the SignalR client code</span></span>

* <span data-ttu-id="c648c-188">Sostituire il codice in *Pages\Index.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c648c-188">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="c648c-189">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="c648c-189">The preceding code:</span></span>

  * <span data-ttu-id="c648c-190">Crea le caselle di testo per il nome e il messaggio di testo, nonché un pulsante di invio.</span><span class="sxs-lookup"><span data-stu-id="c648c-190">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="c648c-191">Crea un elenco con `id="messagesList"` per visualizzare i messaggi ricevuti dall'hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="c648c-191">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="c648c-192">Include i riferimenti agli script in SignalR e il codice dell'applicazione *chat.js* creato nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="c648c-192">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="c648c-193">Nella cartella *wwwroot/js* creare un file *chat.js* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c648c-193">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="c648c-194">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="c648c-194">The preceding code:</span></span>

  * <span data-ttu-id="c648c-195">Crea e avvia una connessione.</span><span class="sxs-lookup"><span data-stu-id="c648c-195">Creates and starts a connection.</span></span>
  * <span data-ttu-id="c648c-196">Aggiunge al pulsante di invio un gestore che invia messaggi all'hub.</span><span class="sxs-lookup"><span data-stu-id="c648c-196">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="c648c-197">Aggiunge all'oggetto connessione un gestore che riceve i messaggi dall'hub e li aggiunge all'elenco.</span><span class="sxs-lookup"><span data-stu-id="c648c-197">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="c648c-198">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="c648c-198">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c648c-199">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c648c-199">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c648c-200">Premere **CTRL+F5** per eseguire l'app senza debug.</span><span class="sxs-lookup"><span data-stu-id="c648c-200">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c648c-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c648c-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c648c-202">Premere **CTRL+F5** per eseguire l'app senza debug.</span><span class="sxs-lookup"><span data-stu-id="c648c-202">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c648c-203">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="c648c-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c648c-204">Nel menu selezionare **Esegui > Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="c648c-204">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="c648c-205">Copiare l'URL dalla barra degli indirizzi, aprire un'altra istanza o scheda del browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="c648c-205">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="c648c-206">Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="c648c-206">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="c648c-207">Nome e messaggio vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="c648c-207">The name and message are displayed on both pages instantly.</span></span>

  ![App SignalR di esempio](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="c648c-209">Se l'app non funziona, aprire gli strumenti di sviluppo (F12) del browser e passare alla console.</span><span class="sxs-lookup"><span data-stu-id="c648c-209">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="c648c-210">È possibile che vengano visualizzati errori correlati al codice HTML e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c648c-210">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="c648c-211">Ad esempio, si supponga di aver inserito *signalr.js* in una cartella diversa da quella indicata nelle istruzioni.</span><span class="sxs-lookup"><span data-stu-id="c648c-211">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="c648c-212">In questo caso il riferimento a tale file non funzionerà e verrà visualizzato un errore 404 nella console.</span><span class="sxs-lookup"><span data-stu-id="c648c-212">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="c648c-213">![Errore di signalr.js non trovato](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="c648c-213">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="c648c-214">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="c648c-214">Next steps</span></span>

<span data-ttu-id="c648c-215">Se si vuole che i client si connettano a un'app SignalR da domini diversi, è necessario abilitare Condivisione risorse tra le origini (CORS).</span><span class="sxs-lookup"><span data-stu-id="c648c-215">If you want clients to connect to a SignalR app from different domains, you have to enable Cross-Origin Resource Sharing (CORS).</span></span> <span data-ttu-id="c648c-216">Per altre informazioni, vedere [Condivisione risorse tra le origini](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span><span class="sxs-lookup"><span data-stu-id="c648c-216">For more information, see [Cross-origin resource sharing](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).</span></span>

<span data-ttu-id="c648c-217">Per altre informazioni su SignalR, hub e client JavaScript, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="c648c-217">To learn more about SignalR, hubs, and JavaScript clients, see these resources:</span></span>

* [<span data-ttu-id="c648c-218">Introduzione a SignalR per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c648c-218">Introduction to SignalR for ASP.NET Core</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="c648c-219">Usare gli hub in SignalR per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c648c-219">Use hubs in SignalR for ASP.NET Core</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="c648c-220">Client JavaScript di SignalR per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c648c-220">ASP.NET Core SignalR JavaScript client</span></span>](xref:signalr/javascript-client)
