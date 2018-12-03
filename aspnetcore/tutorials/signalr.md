---
title: Introduzione ad ASP.NET Core SignalR
author: tdykstra
description: In questa esercitazione viene creata un'app di chat che usa ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/13/2018
uid: tutorials/signalr
ms.openlocfilehash: 190717dc6e6f9f2766ba92aa7472f4cdea9b6827
ms.sourcegitcommit: e7fafb153b9de7595c2558a0133f8d1c33a3bddb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2018
ms.locfileid: "52458530"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="80788-103">Esercitazione: Introduzione ad ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="80788-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="80788-104">Questa esercitazione illustra le nozioni di base per compilare un'app in tempo reale con SignalR.</span><span class="sxs-lookup"><span data-stu-id="80788-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="80788-105">Vengono illustrate le seguenti procedure:</span><span class="sxs-lookup"><span data-stu-id="80788-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="80788-106">Creare un progetto Web.</span><span class="sxs-lookup"><span data-stu-id="80788-106">Create a web project.</span></span>
> * <span data-ttu-id="80788-107">Aggiungere la libreria client di SignalR.</span><span class="sxs-lookup"><span data-stu-id="80788-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="80788-108">Creare un hub di SignalR.</span><span class="sxs-lookup"><span data-stu-id="80788-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="80788-109">Configurare il progetto per l'uso di SignalR.</span><span class="sxs-lookup"><span data-stu-id="80788-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="80788-110">Aggiungere codice che invia messaggi da qualsiasi client a tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="80788-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="80788-111">Al termine, si disporrà di un'app di chat funzionante:</span><span class="sxs-lookup"><span data-stu-id="80788-111">At the end, you'll have a working chat app:</span></span>

![App SignalR di esempio](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="80788-113">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="80788-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

> [!NOTE]
> <span data-ttu-id="80788-114">È in corso un test di usabilità di una nuova proposta di struttura del sommario di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="80788-114">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="80788-115">Se si dispone di alcuni minuti per provare a cercare 7 argomenti diversi nel sommario corrente o proposto, [fare clic qui per partecipare al test](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span><span class="sxs-lookup"><span data-stu-id="80788-115">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="80788-116">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="80788-116">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="80788-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="80788-117">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="80788-118">[Visual Studio 2017](https://www.visualstudio.com/downloads/) versione 15.8 o successive con il carico di lavoro **Sviluppo ASP.NET e Web**</span><span class="sxs-lookup"><span data-stu-id="80788-118">[Visual Studio 2017 version 15.8 or later](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="80788-119">.NET Core SDK 2.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="80788-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="80788-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="80788-120">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="80788-121">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="80788-121">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="80788-122">.NET Core SDK 2.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="80788-122">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="80788-123">C# per Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="80788-123">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="80788-124">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="80788-124">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="80788-125">Visual Studio per Mac versione 7.5.4 o successiva</span><span class="sxs-lookup"><span data-stu-id="80788-125">Visual Studio for Mac version 7.5.4 or later</span></span>](https://www.visualstudio.com/downloads/)
* <span data-ttu-id="80788-126">[.NET Core SDK 2.1 o versione successiva](https://www.microsoft.com/net/download/all) (incluso nell'installazione di Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="80788-126">[.NET Core SDK 2.1 or later](https://www.microsoft.com/net/download/all) (included in the Visual Studio install)</span></span>

---

## <a name="create-a-web-project"></a><span data-ttu-id="80788-127">Creare un progetto Web</span><span class="sxs-lookup"><span data-stu-id="80788-127">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="80788-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="80788-128">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="80788-129">Nel menu selezionare **File > Nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="80788-129">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="80788-130">Nella finestra di dialogo **Nuovo progetto** selezionare **Installate > Visual C# > Web > Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="80788-130">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="80788-131">Denominare il progetto *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="80788-131">Name the project *SignalRChat*.</span></span>

  ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="80788-133">Selezionare **Applicazione Web** per creare un progetto che usa Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="80788-133">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="80788-134">Selezionare un framework di destinazione di **.NET Core**, selezionare **ASP.NET Core 2.1** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="80788-134">Select a target framework of **.NET Core**, select **ASP.NET Core 2.1**, and click **OK**.</span></span>

  ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="80788-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="80788-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="80788-137">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal) alla cartella in cui verrà creata la nuova cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="80788-137">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="80788-138">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="80788-138">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="80788-139">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="80788-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="80788-140">Nel menu selezionare **File > Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="80788-140">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="80788-141">Selezionare **.NET Core > App > App Web ASP.NET Core** (non selezionare **App Web ASP.NET Core (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="80788-141">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="80788-142">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="80788-142">Select **Next**.</span></span>

* <span data-ttu-id="80788-143">Assegnare al progetto il nome *SignalRChat* e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="80788-143">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="80788-144">Aggiungere la libreria client di SignalR</span><span class="sxs-lookup"><span data-stu-id="80788-144">Add the SignalR client library</span></span>

<span data-ttu-id="80788-145">La libreria server di SignalR è inclusa nel metapacchetto `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="80788-145">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="80788-146">La libreria client JavaScript non viene inclusa automaticamente nel progetto.</span><span class="sxs-lookup"><span data-stu-id="80788-146">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="80788-147">Per questa esercitazione, usare Gestione librerie (LibMan) per ottenere la libreria client da *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="80788-147">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="80788-148">Unpkg è una rete per la distribuzione di contenuti (CDN, Content Delivery Network) che può offrire qualsiasi contenuto disponibile in npm, la gestione pacchetti di Node.js.</span><span class="sxs-lookup"><span data-stu-id="80788-148">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="80788-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="80788-149">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="80788-150">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **Client-Side Library** (Libreria lato client).</span><span class="sxs-lookup"><span data-stu-id="80788-150">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="80788-151">Nella finestra di dialogo **Add Client-Side Library** (Aggiungi libreria lato client) selezionare **unpkg** in **Provider**.</span><span class="sxs-lookup"><span data-stu-id="80788-151">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="80788-152">In **Libreria** immettere `@aspnet/signalr@1` e selezionare la versione più recente che non sia di anteprima.</span><span class="sxs-lookup"><span data-stu-id="80788-152">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Finestra di dialogo Add Client-Side Library (Aggiungi libreria lato client) - selezione della libreria](signalr/_static/libman1.png)

* <span data-ttu-id="80788-154">Selezionare **Choose specific files** (Scegli file specifici), espandere la cartella *dist/browser* e selezionare *signalr.js* e *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="80788-154">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="80788-155">Impostare **Percorso di destinazione** su *wwwroot/lib/signalr/* e selezionare **Installa**.</span><span class="sxs-lookup"><span data-stu-id="80788-155">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Finestra di dialogo Add Client-Side Library (Aggiungi libreria lato client) - selezione di file e destinazione](signalr/_static/libman2.png)

  <span data-ttu-id="80788-157">LibMan crea una cartella *wwwroot/lib/signalr* e copia i file selezionati in tale cartella.</span><span class="sxs-lookup"><span data-stu-id="80788-157">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="80788-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="80788-158">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="80788-159">Eseguire il comando seguente nel terminale integrato per installare LibMan.</span><span class="sxs-lookup"><span data-stu-id="80788-159">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="80788-160">Eseguire il comando seguente per ottenere la libreria client di SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="80788-160">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="80788-161">Potrebbe essere necessario attendere alcuni secondi prima di visualizzare l'output.</span><span class="sxs-lookup"><span data-stu-id="80788-161">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="80788-162">I parametri specificano le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="80788-162">The parameters specify the following options:</span></span>
  * <span data-ttu-id="80788-163">Usare il provider unpkg.</span><span class="sxs-lookup"><span data-stu-id="80788-163">Use the unpkg provider.</span></span>
  * <span data-ttu-id="80788-164">Copiare i file nella destinazione *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="80788-164">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="80788-165">Copiare solo i file specificati.</span><span class="sxs-lookup"><span data-stu-id="80788-165">Copy only the specified files.</span></span>

  <span data-ttu-id="80788-166">L'output è simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="80788-166">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="80788-167">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="80788-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="80788-168">Nel **Terminale** eseguire il comando seguente per installare LibMan.</span><span class="sxs-lookup"><span data-stu-id="80788-168">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="80788-169">Passare alla cartella del progetto, ovvero quella che contiene il file *SignalRChat.csproj*.</span><span class="sxs-lookup"><span data-stu-id="80788-169">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="80788-170">Eseguire il comando seguente per ottenere la libreria client di SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="80788-170">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="80788-171">I parametri specificano le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="80788-171">The parameters specify the following options:</span></span>
  * <span data-ttu-id="80788-172">Usare il provider unpkg.</span><span class="sxs-lookup"><span data-stu-id="80788-172">Use the unpkg provider.</span></span>
  * <span data-ttu-id="80788-173">Copiare i file nella destinazione *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="80788-173">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="80788-174">Copiare solo i file specificati.</span><span class="sxs-lookup"><span data-stu-id="80788-174">Copy only the specified files.</span></span>

  <span data-ttu-id="80788-175">L'output è simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="80788-175">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="80788-176">Creare un hub di SignalR</span><span class="sxs-lookup"><span data-stu-id="80788-176">Create a SignalR hub</span></span>

<span data-ttu-id="80788-177">Un *hub* è una classe usata come pipeline di alto livello che gestisce le comunicazioni client-server.</span><span class="sxs-lookup"><span data-stu-id="80788-177">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="80788-178">Nella cartella del progetto SignalRChat creare una cartella *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="80788-178">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="80788-179">Nella cartella *Hubs* creare un file *ChatHub.cs* contenente il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="80788-179">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="80788-180">La classe `ChatHub` eredita dalla classe `Hub` di SignalR.</span><span class="sxs-lookup"><span data-stu-id="80788-180">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="80788-181">La classe `Hub` gestisce connessioni, gruppi e messaggistica.</span><span class="sxs-lookup"><span data-stu-id="80788-181">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="80788-182">Il metodo `SendMessage`, che può essere chiamato da qualsiasi client connesso,</span><span class="sxs-lookup"><span data-stu-id="80788-182">The `SendMessage` method can be called by any connected client.</span></span> <span data-ttu-id="80788-183">invia il messaggio ricevuto a tutti i client.</span><span class="sxs-lookup"><span data-stu-id="80788-183">It sends the received message to all clients.</span></span> <span data-ttu-id="80788-184">Il codice di SignalR è asincrono allo scopo di offrire la massima scalabilità.</span><span class="sxs-lookup"><span data-stu-id="80788-184">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="80788-185">Configurare SignalR</span><span class="sxs-lookup"><span data-stu-id="80788-185">Configure SignalR</span></span>

<span data-ttu-id="80788-186">È necessario configurare il server SignalR in modo che passi le richieste SignalR a SignalR.</span><span class="sxs-lookup"><span data-stu-id="80788-186">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="80788-187">Aggiungere il codice evidenziato seguente al file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="80788-187">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="80788-188">Con queste modifiche SignalR viene aggiunto al sistema di inserimento delle dipendenze di ASP.NET Core e alla pipeline middleware.</span><span class="sxs-lookup"><span data-stu-id="80788-188">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="80788-189">Aggiungere il codice del client SignalR</span><span class="sxs-lookup"><span data-stu-id="80788-189">Add SignalR client code</span></span>

* <span data-ttu-id="80788-190">Sostituire il codice in *Pages\Index.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="80788-190">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="80788-191">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="80788-191">The preceding code:</span></span>

  * <span data-ttu-id="80788-192">Crea le caselle di testo per il nome e il messaggio di testo, nonché un pulsante di invio.</span><span class="sxs-lookup"><span data-stu-id="80788-192">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="80788-193">Crea un elenco con `id="messagesList"` per visualizzare i messaggi ricevuti dall'hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="80788-193">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="80788-194">Include i riferimenti agli script in SignalR e il codice dell'applicazione *chat.js* creato nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="80788-194">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="80788-195">Nella cartella *wwwroot/js* creare un file *chat.js* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="80788-195">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="80788-196">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="80788-196">The preceding code:</span></span>

  * <span data-ttu-id="80788-197">Crea e avvia una connessione.</span><span class="sxs-lookup"><span data-stu-id="80788-197">Creates and starts a connection.</span></span>
  * <span data-ttu-id="80788-198">Aggiunge al pulsante di invio un gestore che invia messaggi all'hub.</span><span class="sxs-lookup"><span data-stu-id="80788-198">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="80788-199">Aggiunge all'oggetto connessione un gestore che riceve i messaggi dall'hub e li aggiunge all'elenco.</span><span class="sxs-lookup"><span data-stu-id="80788-199">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="80788-200">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="80788-200">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="80788-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="80788-201">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="80788-202">Premere **CTRL+F5** per eseguire l'app senza debug.</span><span class="sxs-lookup"><span data-stu-id="80788-202">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="80788-203">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="80788-203">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="80788-204">Eseguire il comando seguente nel terminale integrato:</span><span class="sxs-lookup"><span data-stu-id="80788-204">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="80788-205">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="80788-205">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="80788-206">Nel menu selezionare **Esegui > Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="80788-206">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="80788-207">Copiare l'URL dalla barra degli indirizzi, aprire un'altra istanza o scheda del browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="80788-207">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="80788-208">Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia messaggio**.</span><span class="sxs-lookup"><span data-stu-id="80788-208">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="80788-209">Nome e messaggio vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="80788-209">The name and message are displayed on both pages instantly.</span></span>

  ![App SignalR di esempio](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="80788-211">Se l'app non funziona, aprire gli strumenti di sviluppo (F12) del browser e passare alla console.</span><span class="sxs-lookup"><span data-stu-id="80788-211">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="80788-212">È possibile che vengano visualizzati errori correlati al codice HTML e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="80788-212">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="80788-213">Ad esempio, si supponga di aver inserito *signalr.js* in una cartella diversa da quella indicata nelle istruzioni.</span><span class="sxs-lookup"><span data-stu-id="80788-213">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="80788-214">In questo caso il riferimento a tale file non funzionerà e verrà visualizzato un errore 404 nella console.</span><span class="sxs-lookup"><span data-stu-id="80788-214">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="80788-215">![Errore di signalr.js non trovato](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="80788-215">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="80788-216">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="80788-216">Next steps</span></span>

<span data-ttu-id="80788-217">In questa esercitazione si è appreso come:</span><span class="sxs-lookup"><span data-stu-id="80788-217">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="80788-218">Creare un progetto di app Web.</span><span class="sxs-lookup"><span data-stu-id="80788-218">Create a web app project.</span></span>
> * <span data-ttu-id="80788-219">Aggiungere la libreria client di SignalR.</span><span class="sxs-lookup"><span data-stu-id="80788-219">Add the SignalR client library.</span></span>
> * <span data-ttu-id="80788-220">Creare un hub di SignalR.</span><span class="sxs-lookup"><span data-stu-id="80788-220">Create a SignalR hub.</span></span>
> * <span data-ttu-id="80788-221">Configurare il progetto per l'uso di SignalR.</span><span class="sxs-lookup"><span data-stu-id="80788-221">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="80788-222">Aggiungere il codice che usa l'hub per inviare messaggi a tutti i client connessi da qualsiasi client.</span><span class="sxs-lookup"><span data-stu-id="80788-222">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="80788-223">Per altre informazioni su SignalR, vedere l'introduzione:</span><span class="sxs-lookup"><span data-stu-id="80788-223">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="80788-224">Introduzione a SignalR di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="80788-224">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
