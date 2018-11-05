---
title: Introduzione ad ASP.NET Core SignalR
author: tdykstra
description: In questa esercitazione viene creata un'app di chat che usa ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: fcfe2fa6cc88b9eee1389e171fa5eb7711b4f14f
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758128"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="e4f16-103">Esercitazione: Introduzione ad ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="e4f16-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="e4f16-104">Questa esercitazione illustra le nozioni di base per compilare un'app in tempo reale con SignalR.</span><span class="sxs-lookup"><span data-stu-id="e4f16-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="e4f16-105">Vengono illustrate le seguenti procedure:</span><span class="sxs-lookup"><span data-stu-id="e4f16-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e4f16-106">Creare un progetto Web.</span><span class="sxs-lookup"><span data-stu-id="e4f16-106">Create a web project.</span></span>
> * <span data-ttu-id="e4f16-107">Aggiungere la libreria client di SignalR.</span><span class="sxs-lookup"><span data-stu-id="e4f16-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="e4f16-108">Creare un hub di SignalR.</span><span class="sxs-lookup"><span data-stu-id="e4f16-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="e4f16-109">Configurare il progetto per l'uso di SignalR.</span><span class="sxs-lookup"><span data-stu-id="e4f16-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="e4f16-110">Aggiungere codice che invia messaggi da qualsiasi client a tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="e4f16-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="e4f16-111">Al termine, si disporrà di un'app di chat funzionante:</span><span class="sxs-lookup"><span data-stu-id="e4f16-111">At the end, you'll have a working chat app:</span></span>

![App SignalR di esempio](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="e4f16-113">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="e4f16-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4f16-114">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="e4f16-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4f16-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4f16-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e4f16-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) versione 15.8 o successive con il carico di lavoro **Sviluppo ASP.NET e Web**</span><span class="sxs-lookup"><span data-stu-id="e4f16-116">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="e4f16-117">.NET Core SDK 2.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="e4f16-117">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e4f16-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4f16-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="e4f16-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4f16-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="e4f16-120">.NET Core SDK 2.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="e4f16-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="e4f16-121">C# per Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4f16-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e4f16-122">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e4f16-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="e4f16-123">Visual Studio per Mac versione 7.5.4 o successiva</span><span class="sxs-lookup"><span data-stu-id="e4f16-123">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="e4f16-124">[.NET Core SDK 2.1 o versione successiva](https://www.microsoft.com/net/download/all) (incluso nell'installazione di Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="e4f16-124">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-a-web-project"></a><span data-ttu-id="e4f16-125">Creare un progetto Web</span><span class="sxs-lookup"><span data-stu-id="e4f16-125">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4f16-126">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4f16-126">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="e4f16-127">Nel menu selezionare **File > Nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="e4f16-127">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="e4f16-128">Nella finestra di dialogo **Nuovo progetto** selezionare **Installate > Visual C# > Web > Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e4f16-128">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="e4f16-129">Denominare il progetto *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="e4f16-129">Name the project *SignalRChat*.</span></span>

  ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="e4f16-131">Selezionare **Applicazione Web** per creare un progetto che usa Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="e4f16-131">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="e4f16-132">Selezionare un framework di destinazione di **.NET Core**, selezionare **ASP.NET Core 2.1** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="e4f16-132">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e4f16-134">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4f16-134">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="e4f16-135">Aprire una cartella che è possibile usare per un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="e4f16-135">Open a folder that you can use for a new project.</span></span>

* <span data-ttu-id="e4f16-136">In [Terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal) eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e4f16-136">In the [Integrated Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal), run the following command:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e4f16-137">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e4f16-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e4f16-138">Nel menu selezionare **File > Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="e4f16-138">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="e4f16-139">Selezionare **.NET Core > App > App Web ASP.NET Core** (non selezionare **App Web ASP.NET Core (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="e4f16-139">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="e4f16-140">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="e4f16-140">Select **Next**.</span></span>

* <span data-ttu-id="e4f16-141">Assegnare al progetto il nome *SignalRChat* e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="e4f16-141">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="e4f16-142">Aggiungere la libreria client di SignalR</span><span class="sxs-lookup"><span data-stu-id="e4f16-142">Add the SignalR client library</span></span>

<span data-ttu-id="e4f16-143">La libreria server di SignalR è inclusa nel metapacchetto `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="e4f16-143">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="e4f16-144">La libreria client JavaScript non viene inclusa automaticamente nel progetto.</span><span class="sxs-lookup"><span data-stu-id="e4f16-144">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="e4f16-145">Per questa esercitazione, usare Gestione librerie (LibMan) per ottenere la libreria client da *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="e4f16-145">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="e4f16-146">Unpkg è una rete per la distribuzione di contenuti (CDN, Content Delivery Network) che può offrire qualsiasi contenuto disponibile in npm, la gestione pacchetti di Node.js.</span><span class="sxs-lookup"><span data-stu-id="e4f16-146">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4f16-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4f16-147">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="e4f16-148">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **Client-Side Library** (Libreria lato client).</span><span class="sxs-lookup"><span data-stu-id="e4f16-148">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="e4f16-149">Nella finestra di dialogo **Add Client-Side Library** (Aggiungi libreria lato client) selezionare **unpkg** in **Provider**.</span><span class="sxs-lookup"><span data-stu-id="e4f16-149">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="e4f16-150">In **Libreria** immettere `@aspnet/signalr@1` e selezionare la versione più recente che non sia di anteprima.</span><span class="sxs-lookup"><span data-stu-id="e4f16-150">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Finestra di dialogo Add Client-Side Library (Aggiungi libreria lato client) - selezione della libreria](signalr/_static/libman1.png)

* <span data-ttu-id="e4f16-152">Selezionare **Choose specific files** (Scegli file specifici), espandere la cartella *dist/browser* e selezionare *signalr.js* e *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="e4f16-152">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="e4f16-153">Impostare **Percorso di destinazione** su *wwwroot/lib/signalr/* e selezionare **Installa**.</span><span class="sxs-lookup"><span data-stu-id="e4f16-153">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Finestra di dialogo Add Client-Side Library (Aggiungi libreria lato client) - selezione di file e destinazione](signalr/_static/libman2.png)

  <span data-ttu-id="e4f16-155">LibMan crea una cartella *wwwroot/lib/signalr* e copia i file selezionati in tale cartella.</span><span class="sxs-lookup"><span data-stu-id="e4f16-155">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e4f16-156">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4f16-156">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="e4f16-157">In **Terminale integrato** eseguire il comando seguente per installare LibMan.</span><span class="sxs-lookup"><span data-stu-id="e4f16-157">In the **Integrated Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="e4f16-158">Passare alla cartella del progetto, ovvero quella che contiene il file *SignalRChat.csproj*.</span><span class="sxs-lookup"><span data-stu-id="e4f16-158">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="e4f16-159">Eseguire il comando seguente per ottenere la libreria client di SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="e4f16-159">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="e4f16-160">Potrebbe essere necessario attendere alcuni secondi prima di visualizzare l'output.</span><span class="sxs-lookup"><span data-stu-id="e4f16-160">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="e4f16-161">I parametri specificano le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="e4f16-161">The parameters specify the following options:</span></span>
  * <span data-ttu-id="e4f16-162">Usare il provider unpkg.</span><span class="sxs-lookup"><span data-stu-id="e4f16-162">Use the unpkg provider.</span></span>
  * <span data-ttu-id="e4f16-163">Copiare i file nella destinazione *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="e4f16-163">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="e4f16-164">Copiare solo i file specificati.</span><span class="sxs-lookup"><span data-stu-id="e4f16-164">Copy only the specified files.</span></span>

  <span data-ttu-id="e4f16-165">L'output è simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="e4f16-165">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e4f16-166">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e4f16-166">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e4f16-167">Nel **Terminale** eseguire il comando seguente per installare LibMan.</span><span class="sxs-lookup"><span data-stu-id="e4f16-167">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="e4f16-168">Passare alla cartella del progetto, ovvero quella che contiene il file *SignalRChat.csproj*.</span><span class="sxs-lookup"><span data-stu-id="e4f16-168">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="e4f16-169">Eseguire il comando seguente per ottenere la libreria client di SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="e4f16-169">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="e4f16-170">I parametri specificano le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="e4f16-170">The parameters specify the following options:</span></span>
  * <span data-ttu-id="e4f16-171">Usare il provider unpkg.</span><span class="sxs-lookup"><span data-stu-id="e4f16-171">Use the unpkg provider.</span></span>
  * <span data-ttu-id="e4f16-172">Copiare i file nella destinazione *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="e4f16-172">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="e4f16-173">Copiare solo i file specificati.</span><span class="sxs-lookup"><span data-stu-id="e4f16-173">Copy only the specified files.</span></span>

  <span data-ttu-id="e4f16-174">L'output è simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="e4f16-174">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="e4f16-175">Creare un hub di SignalR</span><span class="sxs-lookup"><span data-stu-id="e4f16-175">Create a SignalR hub</span></span>

<span data-ttu-id="e4f16-176">Un *hub* è una classe usata come pipeline di alto livello che gestisce le comunicazioni client-server.</span><span class="sxs-lookup"><span data-stu-id="e4f16-176">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="e4f16-177">Nella cartella del progetto SignalRChat creare una cartella *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="e4f16-177">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="e4f16-178">Nella cartella *Hubs* creare un file *ChatHub.cs* contenente il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="e4f16-178">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="e4f16-179">La classe `ChatHub` eredita dalla classe `Hub` di SignalR.</span><span class="sxs-lookup"><span data-stu-id="e4f16-179">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="e4f16-180">La classe `Hub` gestisce connessioni, gruppi e messaggistica.</span><span class="sxs-lookup"><span data-stu-id="e4f16-180">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="e4f16-181">Il metodo `SendMessage`, che può essere chiamato da qualsiasi client connesso,</span><span class="sxs-lookup"><span data-stu-id="e4f16-181">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="e4f16-182">invia il messaggio ricevuto a tutti i client.</span><span class="sxs-lookup"><span data-stu-id="e4f16-182">It sends the received message to all clients.</span></span> <span data-ttu-id="e4f16-183">Il codice di SignalR è asincrono allo scopo di offrire la massima scalabilità.</span><span class="sxs-lookup"><span data-stu-id="e4f16-183">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="e4f16-184">Configurare SignalR</span><span class="sxs-lookup"><span data-stu-id="e4f16-184">Configure SignalR</span></span>

<span data-ttu-id="e4f16-185">È necessario configurare il server SignalR in modo che passi le richieste SignalR a SignalR.</span><span class="sxs-lookup"><span data-stu-id="e4f16-185">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="e4f16-186">Aggiungere il codice evidenziato seguente al file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="e4f16-186">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="e4f16-187">Con queste modifiche SignalR viene aggiunto al sistema di inserimento delle dipendenze di ASP.NET Core e alla pipeline middleware.</span><span class="sxs-lookup"><span data-stu-id="e4f16-187">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="e4f16-188">Aggiungere il codice del client SignalR</span><span class="sxs-lookup"><span data-stu-id="e4f16-188">Add SignalR client code</span></span>

* <span data-ttu-id="e4f16-189">Sostituire il codice in *Pages\Index.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="e4f16-189">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="e4f16-190">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="e4f16-190">The preceding code:</span></span>

  * <span data-ttu-id="e4f16-191">Crea le caselle di testo per il nome e il messaggio di testo, nonché un pulsante di invio.</span><span class="sxs-lookup"><span data-stu-id="e4f16-191">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="e4f16-192">Crea un elenco con `id="messagesList"` per visualizzare i messaggi ricevuti dall'hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="e4f16-192">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="e4f16-193">Include i riferimenti agli script in SignalR e il codice dell'applicazione *chat.js* creato nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="e4f16-193">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="e4f16-194">Nella cartella *wwwroot/js* creare un file *chat.js* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="e4f16-194">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="e4f16-195">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="e4f16-195">The preceding code:</span></span>

  * <span data-ttu-id="e4f16-196">Crea e avvia una connessione.</span><span class="sxs-lookup"><span data-stu-id="e4f16-196">Creates and starts a connection.</span></span>
  * <span data-ttu-id="e4f16-197">Aggiunge al pulsante di invio un gestore che invia messaggi all'hub.</span><span class="sxs-lookup"><span data-stu-id="e4f16-197">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="e4f16-198">Aggiunge all'oggetto connessione un gestore che riceve i messaggi dall'hub e li aggiunge all'elenco.</span><span class="sxs-lookup"><span data-stu-id="e4f16-198">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="e4f16-199">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="e4f16-199">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e4f16-200">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4f16-200">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e4f16-201">Premere **CTRL+F5** per eseguire l'app senza debug.</span><span class="sxs-lookup"><span data-stu-id="e4f16-201">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e4f16-202">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4f16-202">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e4f16-203">Premere **CTRL+F5** per eseguire l'app senza debug.</span><span class="sxs-lookup"><span data-stu-id="e4f16-203">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e4f16-204">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e4f16-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e4f16-205">Nel menu selezionare **Esegui > Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="e4f16-205">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="e4f16-206">Copiare l'URL dalla barra degli indirizzi, aprire un'altra istanza o scheda del browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="e4f16-206">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="e4f16-207">Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="e4f16-207">Choose either browser, enter a name and message, and select the **Send** button.</span></span>

  <span data-ttu-id="e4f16-208">Nome e messaggio vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="e4f16-208">The name and message are displayed on both pages instantly.</span></span>

  ![App SignalR di esempio](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="e4f16-210">Se l'app non funziona, aprire gli strumenti di sviluppo (F12) del browser e passare alla console.</span><span class="sxs-lookup"><span data-stu-id="e4f16-210">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="e4f16-211">È possibile che vengano visualizzati errori correlati al codice HTML e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e4f16-211">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="e4f16-212">Ad esempio, si supponga di aver inserito *signalr.js* in una cartella diversa da quella indicata nelle istruzioni.</span><span class="sxs-lookup"><span data-stu-id="e4f16-212">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="e4f16-213">In questo caso il riferimento a tale file non funzionerà e verrà visualizzato un errore 404 nella console.</span><span class="sxs-lookup"><span data-stu-id="e4f16-213">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="e4f16-214">![Errore di signalr.js non trovato](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="e4f16-214">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="e4f16-215">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="e4f16-215">Next steps</span></span>

<span data-ttu-id="e4f16-216">In questa esercitazione si è appreso come:</span><span class="sxs-lookup"><span data-stu-id="e4f16-216">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e4f16-217">Creare un progetto di app Web.</span><span class="sxs-lookup"><span data-stu-id="e4f16-217">Create a web app project.</span></span>
> * <span data-ttu-id="e4f16-218">Aggiungere la libreria client di SignalR.</span><span class="sxs-lookup"><span data-stu-id="e4f16-218">Add the SignalR client library.</span></span>
> * <span data-ttu-id="e4f16-219">Creare un hub di SignalR.</span><span class="sxs-lookup"><span data-stu-id="e4f16-219">Create a SignalR hub.</span></span>
> * <span data-ttu-id="e4f16-220">Configurare il progetto per l'uso di SignalR.</span><span class="sxs-lookup"><span data-stu-id="e4f16-220">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="e4f16-221">Aggiungere il codice che usa l'hub per inviare messaggi a tutti i client connessi da qualsiasi client.</span><span class="sxs-lookup"><span data-stu-id="e4f16-221">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="e4f16-222">Per altre informazioni su SignalR, vedere l'introduzione:</span><span class="sxs-lookup"><span data-stu-id="e4f16-222">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e4f16-223">Introduzione a SignalR di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e4f16-223">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
