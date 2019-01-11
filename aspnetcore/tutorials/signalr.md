---
title: Introduzione ad ASP.NET Core SignalR
author: tdykstra
description: In questa esercitazione viene creata un'app di chat che usa ASP.NET Core SignalR.
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/30/2018
uid: tutorials/signalr
ms.openlocfilehash: 36296513726f7e098a536afc22fcbfb2cafe946d
ms.sourcegitcommit: e1cc4c1ef6c9e07918a609d5ad7fadcb6abe3e12
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/03/2019
ms.locfileid: "53997279"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="01df1-103">Esercitazione: Introduzione ad ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="01df1-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="01df1-104">Questa esercitazione illustra le nozioni di base per compilare un'app in tempo reale con SignalR.</span><span class="sxs-lookup"><span data-stu-id="01df1-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="01df1-105">Vengono illustrate le seguenti procedure:</span><span class="sxs-lookup"><span data-stu-id="01df1-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="01df1-106">Creare un progetto Web.</span><span class="sxs-lookup"><span data-stu-id="01df1-106">Create a web project.</span></span>
> * <span data-ttu-id="01df1-107">Aggiungere la libreria client di SignalR.</span><span class="sxs-lookup"><span data-stu-id="01df1-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="01df1-108">Creare un hub di SignalR.</span><span class="sxs-lookup"><span data-stu-id="01df1-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="01df1-109">Configurare il progetto per l'uso di SignalR.</span><span class="sxs-lookup"><span data-stu-id="01df1-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="01df1-110">Aggiungere codice che invia messaggi da qualsiasi client a tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="01df1-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="01df1-111">Al termine, si disporrà di un'app di chat funzionante:</span><span class="sxs-lookup"><span data-stu-id="01df1-111">At the end, you'll have a working chat app:</span></span>

![App SignalR di esempio](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="01df1-113">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="01df1-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

[!INCLUDE [|Prerequisites](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a><span data-ttu-id="01df1-114">Creare un progetto Web</span><span class="sxs-lookup"><span data-stu-id="01df1-114">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="01df1-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01df1-115">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="01df1-116">Nel menu selezionare **File > Nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="01df1-116">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="01df1-117">Nella finestra di dialogo **Nuovo progetto** selezionare **Installate > Visual C# > Web > Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="01df1-117">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="01df1-118">Denominare il progetto *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="01df1-118">Name the project *SignalRChat*.</span></span>

  ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="01df1-120">Selezionare **Applicazione Web** per creare un progetto che usa Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="01df1-120">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="01df1-121">Selezionare un framework di destinazione di **.NET Core**, selezionare **ASP.NET Core 2.2** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="01df1-121">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>

  ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="01df1-123">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="01df1-123">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="01df1-124">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal) alla cartella in cui verrà creata la nuova cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="01df1-124">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="01df1-125">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="01df1-125">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="01df1-126">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="01df1-126">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="01df1-127">Nel menu selezionare **File > Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="01df1-127">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="01df1-128">Selezionare **.NET Core > App > App Web ASP.NET Core** (non selezionare **App Web ASP.NET Core (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="01df1-128">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="01df1-129">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="01df1-129">Select **Next**.</span></span>

* <span data-ttu-id="01df1-130">Assegnare al progetto il nome *SignalRChat* e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="01df1-130">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="01df1-131">Aggiungere la libreria client di SignalR</span><span class="sxs-lookup"><span data-stu-id="01df1-131">Add the SignalR client library</span></span>

<span data-ttu-id="01df1-132">La libreria server di SignalR è inclusa nel metapacchetto `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="01df1-132">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="01df1-133">La libreria client JavaScript non viene inclusa automaticamente nel progetto.</span><span class="sxs-lookup"><span data-stu-id="01df1-133">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="01df1-134">Per questa esercitazione, usare Gestione librerie (LibMan) per ottenere la libreria client da *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="01df1-134">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="01df1-135">Unpkg è una rete per la distribuzione di contenuti (CDN, Content Delivery Network) che può offrire qualsiasi contenuto disponibile in npm, la gestione pacchetti di Node.js.</span><span class="sxs-lookup"><span data-stu-id="01df1-135">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="01df1-136">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01df1-136">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="01df1-137">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **Client-Side Library** (Libreria lato client).</span><span class="sxs-lookup"><span data-stu-id="01df1-137">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="01df1-138">Nella finestra di dialogo **Add Client-Side Library** (Aggiungi libreria lato client) selezionare **unpkg** in **Provider**.</span><span class="sxs-lookup"><span data-stu-id="01df1-138">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="01df1-139">In **Libreria** immettere `@aspnet/signalr@1` e selezionare la versione più recente che non sia di anteprima.</span><span class="sxs-lookup"><span data-stu-id="01df1-139">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Finestra di dialogo Add Client-Side Library (Aggiungi libreria lato client) - selezione della libreria](signalr/_static/libman1.png)

* <span data-ttu-id="01df1-141">Selezionare **Choose specific files** (Scegli file specifici), espandere la cartella *dist/browser* e selezionare *signalr.js* e *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="01df1-141">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="01df1-142">Impostare **Percorso di destinazione** su *wwwroot/lib/signalr/* e selezionare **Installa**.</span><span class="sxs-lookup"><span data-stu-id="01df1-142">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Finestra di dialogo Add Client-Side Library (Aggiungi libreria lato client) - selezione di file e destinazione](signalr/_static/libman2.png)

  <span data-ttu-id="01df1-144">LibMan crea una cartella *wwwroot/lib/signalr* e copia i file selezionati in tale cartella.</span><span class="sxs-lookup"><span data-stu-id="01df1-144">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="01df1-145">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="01df1-145">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="01df1-146">Eseguire il comando seguente nel terminale integrato per installare LibMan.</span><span class="sxs-lookup"><span data-stu-id="01df1-146">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="01df1-147">Eseguire il comando seguente per ottenere la libreria client di SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="01df1-147">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="01df1-148">Potrebbe essere necessario attendere alcuni secondi prima di visualizzare l'output.</span><span class="sxs-lookup"><span data-stu-id="01df1-148">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="01df1-149">I parametri specificano le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="01df1-149">The parameters specify the following options:</span></span>
  * <span data-ttu-id="01df1-150">Usare il provider unpkg.</span><span class="sxs-lookup"><span data-stu-id="01df1-150">Use the unpkg provider.</span></span>
  * <span data-ttu-id="01df1-151">Copiare i file nella destinazione *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="01df1-151">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="01df1-152">Copiare solo i file specificati.</span><span class="sxs-lookup"><span data-stu-id="01df1-152">Copy only the specified files.</span></span>

  <span data-ttu-id="01df1-153">L'output è simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="01df1-153">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="01df1-154">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="01df1-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="01df1-155">Nel **Terminale** eseguire il comando seguente per installare LibMan.</span><span class="sxs-lookup"><span data-stu-id="01df1-155">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="01df1-156">Passare alla cartella del progetto, ovvero quella che contiene il file *SignalRChat.csproj*.</span><span class="sxs-lookup"><span data-stu-id="01df1-156">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="01df1-157">Eseguire il comando seguente per ottenere la libreria client di SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="01df1-157">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="01df1-158">I parametri specificano le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="01df1-158">The parameters specify the following options:</span></span>
  * <span data-ttu-id="01df1-159">Usare il provider unpkg.</span><span class="sxs-lookup"><span data-stu-id="01df1-159">Use the unpkg provider.</span></span>
  * <span data-ttu-id="01df1-160">Copiare i file nella destinazione *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="01df1-160">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="01df1-161">Copiare solo i file specificati.</span><span class="sxs-lookup"><span data-stu-id="01df1-161">Copy only the specified files.</span></span>

  <span data-ttu-id="01df1-162">L'output è simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="01df1-162">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="01df1-163">Creare un hub di SignalR</span><span class="sxs-lookup"><span data-stu-id="01df1-163">Create a SignalR hub</span></span>

<span data-ttu-id="01df1-164">Un *hub* è una classe usata come pipeline di alto livello che gestisce le comunicazioni client-server.</span><span class="sxs-lookup"><span data-stu-id="01df1-164">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="01df1-165">Nella cartella del progetto SignalRChat creare una cartella *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="01df1-165">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="01df1-166">Nella cartella *Hubs* creare un file *ChatHub.cs* contenente il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="01df1-166">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="01df1-167">La classe `ChatHub` eredita dalla classe `Hub` di SignalR.</span><span class="sxs-lookup"><span data-stu-id="01df1-167">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="01df1-168">La classe `Hub` gestisce connessioni, gruppi e messaggistica.</span><span class="sxs-lookup"><span data-stu-id="01df1-168">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="01df1-169">Il metodo `SendMessage` può essere chiamato da un client connesso per inviare un messaggio a tutti i client.</span><span class="sxs-lookup"><span data-stu-id="01df1-169">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="01df1-170">Il codice client JavaScript che chiama il metodo è illustrato più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="01df1-170">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="01df1-171">Il codice di SignalR è asincrono allo scopo di offrire la massima scalabilità.</span><span class="sxs-lookup"><span data-stu-id="01df1-171">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="01df1-172">Configurare SignalR</span><span class="sxs-lookup"><span data-stu-id="01df1-172">Configure SignalR</span></span>

<span data-ttu-id="01df1-173">È necessario configurare il server SignalR in modo che passi le richieste SignalR a SignalR.</span><span class="sxs-lookup"><span data-stu-id="01df1-173">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="01df1-174">Aggiungere il codice evidenziato seguente al file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="01df1-174">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="01df1-175">Con queste modifiche SignalR viene aggiunto al sistema di inserimento delle dipendenze di ASP.NET Core e alla pipeline middleware.</span><span class="sxs-lookup"><span data-stu-id="01df1-175">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="01df1-176">Aggiungere il codice del client SignalR</span><span class="sxs-lookup"><span data-stu-id="01df1-176">Add SignalR client code</span></span>

* <span data-ttu-id="01df1-177">Sostituire il codice in *Pages\Index.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="01df1-177">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="01df1-178">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="01df1-178">The preceding code:</span></span>

  * <span data-ttu-id="01df1-179">Crea le caselle di testo per il nome e il messaggio di testo, nonché un pulsante di invio.</span><span class="sxs-lookup"><span data-stu-id="01df1-179">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="01df1-180">Crea un elenco con `id="messagesList"` per visualizzare i messaggi ricevuti dall'hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="01df1-180">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="01df1-181">Include i riferimenti agli script in SignalR e il codice dell'applicazione *chat.js* creato nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="01df1-181">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="01df1-182">Nella cartella *wwwroot/js* creare un file *chat.js* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="01df1-182">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="01df1-183">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="01df1-183">The preceding code:</span></span>

  * <span data-ttu-id="01df1-184">Crea e avvia una connessione.</span><span class="sxs-lookup"><span data-stu-id="01df1-184">Creates and starts a connection.</span></span>
  * <span data-ttu-id="01df1-185">Aggiunge al pulsante di invio un gestore che invia messaggi all'hub.</span><span class="sxs-lookup"><span data-stu-id="01df1-185">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="01df1-186">Aggiunge all'oggetto connessione un gestore che riceve i messaggi dall'hub e li aggiunge all'elenco.</span><span class="sxs-lookup"><span data-stu-id="01df1-186">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="01df1-187">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="01df1-187">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="01df1-188">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01df1-188">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="01df1-189">Premere **CTRL+F5** per eseguire l'app senza debug.</span><span class="sxs-lookup"><span data-stu-id="01df1-189">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="01df1-190">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="01df1-190">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="01df1-191">Eseguire il comando seguente nel terminale integrato:</span><span class="sxs-lookup"><span data-stu-id="01df1-191">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="01df1-192">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="01df1-192">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="01df1-193">Nel menu selezionare **Esegui > Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="01df1-193">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="01df1-194">Copiare l'URL dalla barra degli indirizzi, aprire un'altra istanza o scheda del browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="01df1-194">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="01df1-195">Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia messaggio**.</span><span class="sxs-lookup"><span data-stu-id="01df1-195">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="01df1-196">Nome e messaggio vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="01df1-196">The name and message are displayed on both pages instantly.</span></span>

  ![App SignalR di esempio](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="01df1-198">Se l'app non funziona, aprire gli strumenti di sviluppo (F12) del browser e passare alla console.</span><span class="sxs-lookup"><span data-stu-id="01df1-198">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="01df1-199">È possibile che vengano visualizzati errori correlati al codice HTML e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="01df1-199">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="01df1-200">Ad esempio, si supponga di aver inserito *signalr.js* in una cartella diversa da quella indicata nelle istruzioni.</span><span class="sxs-lookup"><span data-stu-id="01df1-200">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="01df1-201">In questo caso il riferimento a tale file non funzionerà e verrà visualizzato un errore 404 nella console.</span><span class="sxs-lookup"><span data-stu-id="01df1-201">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="01df1-202">![Errore di signalr.js non trovato](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="01df1-202">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="01df1-203">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="01df1-203">Next steps</span></span>

<span data-ttu-id="01df1-204">In questa esercitazione si è appreso come:</span><span class="sxs-lookup"><span data-stu-id="01df1-204">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="01df1-205">Creare un progetto di app Web.</span><span class="sxs-lookup"><span data-stu-id="01df1-205">Create a web app project.</span></span>
> * <span data-ttu-id="01df1-206">Aggiungere la libreria client di SignalR.</span><span class="sxs-lookup"><span data-stu-id="01df1-206">Add the SignalR client library.</span></span>
> * <span data-ttu-id="01df1-207">Creare un hub di SignalR.</span><span class="sxs-lookup"><span data-stu-id="01df1-207">Create a SignalR hub.</span></span>
> * <span data-ttu-id="01df1-208">Configurare il progetto per l'uso di SignalR.</span><span class="sxs-lookup"><span data-stu-id="01df1-208">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="01df1-209">Aggiungere il codice che usa l'hub per inviare messaggi a tutti i client connessi da qualsiasi client.</span><span class="sxs-lookup"><span data-stu-id="01df1-209">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="01df1-210">Per altre informazioni su SignalR, vedere l'introduzione:</span><span class="sxs-lookup"><span data-stu-id="01df1-210">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="01df1-211">Introduzione a SignalR di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="01df1-211">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
