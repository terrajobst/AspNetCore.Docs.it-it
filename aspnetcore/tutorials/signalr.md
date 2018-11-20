---
title: Introduzione ad ASP.NET Core SignalR
author: tdykstra
description: In questa esercitazione viene creata un'app di chat che usa ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/13/2018
uid: tutorials/signalr
ms.openlocfilehash: 8916b3659250c1bcbbc2dc9b3d466586f98bcc7e
ms.sourcegitcommit: d3392f688cfebc1f25616da7489664d69c6ee330
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/16/2018
ms.locfileid: "51818382"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="63393-103">Esercitazione: Introduzione ad ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="63393-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="63393-104">Questa esercitazione illustra le nozioni di base per compilare un'app in tempo reale con SignalR.</span><span class="sxs-lookup"><span data-stu-id="63393-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="63393-105">Vengono illustrate le seguenti procedure:</span><span class="sxs-lookup"><span data-stu-id="63393-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="63393-106">Creare un progetto Web.</span><span class="sxs-lookup"><span data-stu-id="63393-106">Create a web project.</span></span>
> * <span data-ttu-id="63393-107">Aggiungere la libreria client di SignalR.</span><span class="sxs-lookup"><span data-stu-id="63393-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="63393-108">Creare un hub di SignalR.</span><span class="sxs-lookup"><span data-stu-id="63393-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="63393-109">Configurare il progetto per l'uso di SignalR.</span><span class="sxs-lookup"><span data-stu-id="63393-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="63393-110">Aggiungere codice che invia messaggi da qualsiasi client a tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="63393-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="63393-111">Al termine, si disporrà di un'app di chat funzionante:</span><span class="sxs-lookup"><span data-stu-id="63393-111">At the end, you'll have a working chat app:</span></span>

![App SignalR di esempio](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="63393-113">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="63393-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="63393-114">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="63393-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="63393-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63393-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="63393-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) versione 15.8 o successive con il carico di lavoro **Sviluppo ASP.NET e Web**</span><span class="sxs-lookup"><span data-stu-id="63393-116">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="63393-117">.NET Core SDK 2.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="63393-117">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="63393-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="63393-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="63393-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="63393-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="63393-120">.NET Core SDK 2.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="63393-120">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="63393-121">C# per Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="63393-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="63393-122">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="63393-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="63393-123">Visual Studio per Mac versione 7.5.4 o successiva</span><span class="sxs-lookup"><span data-stu-id="63393-123">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="63393-124">[.NET Core SDK 2.1 o versione successiva](https://www.microsoft.com/net/download/all) (incluso nell'installazione di Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="63393-124">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-a-web-project"></a><span data-ttu-id="63393-125">Creare un progetto Web</span><span class="sxs-lookup"><span data-stu-id="63393-125">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="63393-126">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63393-126">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="63393-127">Nel menu selezionare **File > Nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="63393-127">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="63393-128">Nella finestra di dialogo **Nuovo progetto** selezionare **Installate > Visual C# > Web > Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="63393-128">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="63393-129">Denominare il progetto *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="63393-129">Name the project *SignalRChat*.</span></span>

  ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="63393-131">Selezionare **Applicazione Web** per creare un progetto che usa Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="63393-131">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="63393-132">Selezionare un framework di destinazione di **.NET Core**, selezionare **ASP.NET Core 2.1** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="63393-132">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="63393-134">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="63393-134">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="63393-135">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal) alla cartella in cui verrà creata la nuova cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="63393-135">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="63393-136">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="63393-136">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="63393-137">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="63393-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="63393-138">Nel menu selezionare **File > Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="63393-138">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="63393-139">Selezionare **.NET Core > App > App Web ASP.NET Core** (non selezionare **App Web ASP.NET Core (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="63393-139">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="63393-140">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="63393-140">Select **Next**.</span></span>

* <span data-ttu-id="63393-141">Assegnare al progetto il nome *SignalRChat* e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="63393-141">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="63393-142">Aggiungere la libreria client di SignalR</span><span class="sxs-lookup"><span data-stu-id="63393-142">Add the SignalR client library</span></span>

<span data-ttu-id="63393-143">La libreria server di SignalR è inclusa nel metapacchetto `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="63393-143">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="63393-144">La libreria client JavaScript non viene inclusa automaticamente nel progetto.</span><span class="sxs-lookup"><span data-stu-id="63393-144">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="63393-145">Per questa esercitazione, usare Gestione librerie (LibMan) per ottenere la libreria client da *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="63393-145">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="63393-146">Unpkg è una rete per la distribuzione di contenuti (CDN, Content Delivery Network) che può offrire qualsiasi contenuto disponibile in npm, la gestione pacchetti di Node.js.</span><span class="sxs-lookup"><span data-stu-id="63393-146">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="63393-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63393-147">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="63393-148">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **Client-Side Library** (Libreria lato client).</span><span class="sxs-lookup"><span data-stu-id="63393-148">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="63393-149">Nella finestra di dialogo **Add Client-Side Library** (Aggiungi libreria lato client) selezionare **unpkg** in **Provider**.</span><span class="sxs-lookup"><span data-stu-id="63393-149">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="63393-150">In **Libreria** immettere `@aspnet/signalr@1` e selezionare la versione più recente che non sia di anteprima.</span><span class="sxs-lookup"><span data-stu-id="63393-150">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Finestra di dialogo Add Client-Side Library (Aggiungi libreria lato client) - selezione della libreria](signalr/_static/libman1.png)

* <span data-ttu-id="63393-152">Selezionare **Choose specific files** (Scegli file specifici), espandere la cartella *dist/browser* e selezionare *signalr.js* e *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="63393-152">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="63393-153">Impostare **Percorso di destinazione** su *wwwroot/lib/signalr/* e selezionare **Installa**.</span><span class="sxs-lookup"><span data-stu-id="63393-153">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Finestra di dialogo Add Client-Side Library (Aggiungi libreria lato client) - selezione di file e destinazione](signalr/_static/libman2.png)

  <span data-ttu-id="63393-155">LibMan crea una cartella *wwwroot/lib/signalr* e copia i file selezionati in tale cartella.</span><span class="sxs-lookup"><span data-stu-id="63393-155">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="63393-156">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="63393-156">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="63393-157">Eseguire il comando seguente nel terminale integrato per installare LibMan.</span><span class="sxs-lookup"><span data-stu-id="63393-157">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="63393-158">Eseguire il comando seguente per ottenere la libreria client di SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="63393-158">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="63393-159">Potrebbe essere necessario attendere alcuni secondi prima di visualizzare l'output.</span><span class="sxs-lookup"><span data-stu-id="63393-159">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="63393-160">I parametri specificano le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="63393-160">The parameters specify the following options:</span></span>
  * <span data-ttu-id="63393-161">Usare il provider unpkg.</span><span class="sxs-lookup"><span data-stu-id="63393-161">Use the unpkg provider.</span></span>
  * <span data-ttu-id="63393-162">Copiare i file nella destinazione *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="63393-162">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="63393-163">Copiare solo i file specificati.</span><span class="sxs-lookup"><span data-stu-id="63393-163">Copy only the specified files.</span></span>

  <span data-ttu-id="63393-164">L'output è simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="63393-164">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="63393-165">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="63393-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="63393-166">Nel **Terminale** eseguire il comando seguente per installare LibMan.</span><span class="sxs-lookup"><span data-stu-id="63393-166">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="63393-167">Passare alla cartella del progetto, ovvero quella che contiene il file *SignalRChat.csproj*.</span><span class="sxs-lookup"><span data-stu-id="63393-167">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="63393-168">Eseguire il comando seguente per ottenere la libreria client di SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="63393-168">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="63393-169">I parametri specificano le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="63393-169">The parameters specify the following options:</span></span>
  * <span data-ttu-id="63393-170">Usare il provider unpkg.</span><span class="sxs-lookup"><span data-stu-id="63393-170">Use the unpkg provider.</span></span>
  * <span data-ttu-id="63393-171">Copiare i file nella destinazione *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="63393-171">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="63393-172">Copiare solo i file specificati.</span><span class="sxs-lookup"><span data-stu-id="63393-172">Copy only the specified files.</span></span>

  <span data-ttu-id="63393-173">L'output è simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="63393-173">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="63393-174">Creare un hub di SignalR</span><span class="sxs-lookup"><span data-stu-id="63393-174">Create a SignalR hub</span></span>

<span data-ttu-id="63393-175">Un *hub* è una classe usata come pipeline di alto livello che gestisce le comunicazioni client-server.</span><span class="sxs-lookup"><span data-stu-id="63393-175">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="63393-176">Nella cartella del progetto SignalRChat creare una cartella *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="63393-176">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="63393-177">Nella cartella *Hubs* creare un file *ChatHub.cs* contenente il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="63393-177">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="63393-178">La classe `ChatHub` eredita dalla classe `Hub` di SignalR.</span><span class="sxs-lookup"><span data-stu-id="63393-178">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="63393-179">La classe `Hub` gestisce connessioni, gruppi e messaggistica.</span><span class="sxs-lookup"><span data-stu-id="63393-179">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="63393-180">Il metodo `SendMessage`, che può essere chiamato da qualsiasi client connesso,</span><span class="sxs-lookup"><span data-stu-id="63393-180">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="63393-181">invia il messaggio ricevuto a tutti i client.</span><span class="sxs-lookup"><span data-stu-id="63393-181">It sends the received message to all clients.</span></span> <span data-ttu-id="63393-182">Il codice di SignalR è asincrono allo scopo di offrire la massima scalabilità.</span><span class="sxs-lookup"><span data-stu-id="63393-182">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="63393-183">Configurare SignalR</span><span class="sxs-lookup"><span data-stu-id="63393-183">Configure SignalR</span></span>

<span data-ttu-id="63393-184">È necessario configurare il server SignalR in modo che passi le richieste SignalR a SignalR.</span><span class="sxs-lookup"><span data-stu-id="63393-184">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="63393-185">Aggiungere il codice evidenziato seguente al file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="63393-185">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="63393-186">Con queste modifiche SignalR viene aggiunto al sistema di inserimento delle dipendenze di ASP.NET Core e alla pipeline middleware.</span><span class="sxs-lookup"><span data-stu-id="63393-186">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="63393-187">Aggiungere il codice del client SignalR</span><span class="sxs-lookup"><span data-stu-id="63393-187">Add SignalR client code</span></span>

* <span data-ttu-id="63393-188">Sostituire il codice in *Pages\Index.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="63393-188">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="63393-189">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="63393-189">The preceding code:</span></span>

  * <span data-ttu-id="63393-190">Crea le caselle di testo per il nome e il messaggio di testo, nonché un pulsante di invio.</span><span class="sxs-lookup"><span data-stu-id="63393-190">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="63393-191">Crea un elenco con `id="messagesList"` per visualizzare i messaggi ricevuti dall'hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="63393-191">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="63393-192">Include i riferimenti agli script in SignalR e il codice dell'applicazione *chat.js* creato nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="63393-192">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="63393-193">Nella cartella *wwwroot/js* creare un file *chat.js* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="63393-193">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="63393-194">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="63393-194">The preceding code:</span></span>

  * <span data-ttu-id="63393-195">Crea e avvia una connessione.</span><span class="sxs-lookup"><span data-stu-id="63393-195">Creates and starts a connection.</span></span>
  * <span data-ttu-id="63393-196">Aggiunge al pulsante di invio un gestore che invia messaggi all'hub.</span><span class="sxs-lookup"><span data-stu-id="63393-196">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="63393-197">Aggiunge all'oggetto connessione un gestore che riceve i messaggi dall'hub e li aggiunge all'elenco.</span><span class="sxs-lookup"><span data-stu-id="63393-197">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="63393-198">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="63393-198">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="63393-199">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63393-199">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="63393-200">Premere **CTRL+F5** per eseguire l'app senza debug.</span><span class="sxs-lookup"><span data-stu-id="63393-200">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="63393-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="63393-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="63393-202">Eseguire il comando seguente nel terminale integrato:</span><span class="sxs-lookup"><span data-stu-id="63393-202">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat
  ```
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="63393-203">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="63393-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="63393-204">Nel menu selezionare **Esegui > Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="63393-204">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="63393-205">Copiare l'URL dalla barra degli indirizzi, aprire un'altra istanza o scheda del browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="63393-205">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="63393-206">Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia messaggio**.</span><span class="sxs-lookup"><span data-stu-id="63393-206">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="63393-207">Nome e messaggio vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="63393-207">The name and message are displayed on both pages instantly.</span></span>

  ![App SignalR di esempio](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="63393-209">Se l'app non funziona, aprire gli strumenti di sviluppo (F12) del browser e passare alla console.</span><span class="sxs-lookup"><span data-stu-id="63393-209">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="63393-210">È possibile che vengano visualizzati errori correlati al codice HTML e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="63393-210">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="63393-211">Ad esempio, si supponga di aver inserito *signalr.js* in una cartella diversa da quella indicata nelle istruzioni.</span><span class="sxs-lookup"><span data-stu-id="63393-211">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="63393-212">In questo caso il riferimento a tale file non funzionerà e verrà visualizzato un errore 404 nella console.</span><span class="sxs-lookup"><span data-stu-id="63393-212">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="63393-213">![Errore di signalr.js non trovato](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="63393-213">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="63393-214">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="63393-214">Next steps</span></span>

<span data-ttu-id="63393-215">In questa esercitazione si è appreso come:</span><span class="sxs-lookup"><span data-stu-id="63393-215">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="63393-216">Creare un progetto di app Web.</span><span class="sxs-lookup"><span data-stu-id="63393-216">Create a web app project.</span></span>
> * <span data-ttu-id="63393-217">Aggiungere la libreria client di SignalR.</span><span class="sxs-lookup"><span data-stu-id="63393-217">Add the SignalR client library.</span></span>
> * <span data-ttu-id="63393-218">Creare un hub di SignalR.</span><span class="sxs-lookup"><span data-stu-id="63393-218">Create a SignalR hub.</span></span>
> * <span data-ttu-id="63393-219">Configurare il progetto per l'uso di SignalR.</span><span class="sxs-lookup"><span data-stu-id="63393-219">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="63393-220">Aggiungere il codice che usa l'hub per inviare messaggi a tutti i client connessi da qualsiasi client.</span><span class="sxs-lookup"><span data-stu-id="63393-220">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="63393-221">Per altre informazioni su SignalR, vedere l'introduzione:</span><span class="sxs-lookup"><span data-stu-id="63393-221">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="63393-222">Introduzione a SignalR di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="63393-222">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
