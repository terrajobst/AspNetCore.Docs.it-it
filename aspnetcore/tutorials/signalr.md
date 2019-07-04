---
title: Introduzione ad ASP.NET Core SignalR
author: bradygaster
description: In questa esercitazione viene creata un'app di chat che usa ASP.NET Core SignalR.
ms.author: bradyg
ms.custom: mvc
ms.date: 11/30/2018
uid: tutorials/signalr
ms.openlocfilehash: 9a4296550a17ac2c348f2406e9f5b39877b02b59
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555929"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="401c3-103">Esercitazione: Introduzione ad ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="401c3-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="401c3-104">Questa esercitazione illustra le nozioni di base per compilare un'app in tempo reale con SignalR.</span><span class="sxs-lookup"><span data-stu-id="401c3-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="401c3-105">Vengono illustrate le seguenti procedure:</span><span class="sxs-lookup"><span data-stu-id="401c3-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="401c3-106">Creare un progetto Web.</span><span class="sxs-lookup"><span data-stu-id="401c3-106">Create a web project.</span></span>
> * <span data-ttu-id="401c3-107">Aggiungere la libreria client di SignalR.</span><span class="sxs-lookup"><span data-stu-id="401c3-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="401c3-108">Creare un hub di SignalR.</span><span class="sxs-lookup"><span data-stu-id="401c3-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="401c3-109">Configurare il progetto per l'uso di SignalR.</span><span class="sxs-lookup"><span data-stu-id="401c3-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="401c3-110">Aggiungere codice che invia messaggi da qualsiasi client a tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="401c3-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="401c3-111">Al termine, si disporrà di un'app di chat funzionante:</span><span class="sxs-lookup"><span data-stu-id="401c3-111">At the end, you'll have a working chat app:</span></span>

![App SignalR di esempio](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="401c3-113">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="401c3-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="401c3-114">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="401c3-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="401c3-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="401c3-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2017-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="401c3-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="401c3-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="401c3-117">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="401c3-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="401c3-118">Creare un progetto Web</span><span class="sxs-lookup"><span data-stu-id="401c3-118">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="401c3-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="401c3-119">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="401c3-120">Nel menu selezionare **File > Nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="401c3-120">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="401c3-121">Nella finestra di dialogo **Nuovo progetto** selezionare **Installate > Visual C# > Web > Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="401c3-121">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="401c3-122">Denominare il progetto *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="401c3-122">Name the project *SignalRChat*.</span></span>

  ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="401c3-124">Selezionare **Applicazione Web** per creare un progetto che usa Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="401c3-124">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="401c3-125">Selezionare un framework di destinazione di **.NET Core**, selezionare **ASP.NET Core 2.2** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="401c3-125">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>

  ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="401c3-127">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="401c3-127">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="401c3-128">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal) alla cartella in cui verrà creata la nuova cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="401c3-128">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="401c3-129">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="401c3-129">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="401c3-130">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="401c3-130">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="401c3-131">Nel menu selezionare **File > Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="401c3-131">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="401c3-132">Selezionare **.NET Core > App > App Web ASP.NET Core** (non selezionare **App Web ASP.NET Core (MVC)** ).</span><span class="sxs-lookup"><span data-stu-id="401c3-132">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="401c3-133">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="401c3-133">Select **Next**.</span></span>

* <span data-ttu-id="401c3-134">Assegnare al progetto il nome *SignalRChat* e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="401c3-134">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="401c3-135">Aggiungere la libreria client di SignalR</span><span class="sxs-lookup"><span data-stu-id="401c3-135">Add the SignalR client library</span></span>

<span data-ttu-id="401c3-136">La libreria server di SignalR è inclusa nel metapacchetto `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="401c3-136">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="401c3-137">La libreria client JavaScript non viene inclusa automaticamente nel progetto.</span><span class="sxs-lookup"><span data-stu-id="401c3-137">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="401c3-138">Per questa esercitazione, usare Gestione librerie (LibMan) per ottenere la libreria client da *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="401c3-138">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="401c3-139">Unpkg è una rete per la distribuzione di contenuti (CDN, Content Delivery Network) che può offrire qualsiasi contenuto disponibile in npm, la gestione pacchetti di Node.js.</span><span class="sxs-lookup"><span data-stu-id="401c3-139">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="401c3-140">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="401c3-140">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="401c3-141">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi** > **Client-Side Library** (Libreria lato client).</span><span class="sxs-lookup"><span data-stu-id="401c3-141">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="401c3-142">Nella finestra di dialogo **Add Client-Side Library** (Aggiungi libreria lato client) selezionare **unpkg** in **Provider**.</span><span class="sxs-lookup"><span data-stu-id="401c3-142">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="401c3-143">In **Libreria** immettere `@aspnet/signalr@1` e selezionare la versione più recente che non sia di anteprima.</span><span class="sxs-lookup"><span data-stu-id="401c3-143">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Finestra di dialogo Add Client-Side Library (Aggiungi libreria lato client) - selezione della libreria](signalr/_static/libman1.png)

* <span data-ttu-id="401c3-145">Selezionare **Choose specific files** (Scegli file specifici), espandere la cartella *dist/browser* e selezionare *signalr.js* e *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="401c3-145">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="401c3-146">Impostare **Percorso di destinazione** su *wwwroot/lib/signalr/* e selezionare **Installa**.</span><span class="sxs-lookup"><span data-stu-id="401c3-146">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Finestra di dialogo Add Client-Side Library (Aggiungi libreria lato client) - selezione di file e destinazione](signalr/_static/libman2.png)

  <span data-ttu-id="401c3-148">LibMan crea una cartella *wwwroot/lib/signalr* e copia i file selezionati in tale cartella.</span><span class="sxs-lookup"><span data-stu-id="401c3-148">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="401c3-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="401c3-149">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="401c3-150">Eseguire il comando seguente nel terminale integrato per installare LibMan.</span><span class="sxs-lookup"><span data-stu-id="401c3-150">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="401c3-151">Eseguire il comando seguente per ottenere la libreria client di SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="401c3-151">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="401c3-152">Potrebbe essere necessario attendere alcuni secondi prima di visualizzare l'output.</span><span class="sxs-lookup"><span data-stu-id="401c3-152">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="401c3-153">I parametri specificano le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="401c3-153">The parameters specify the following options:</span></span>
  * <span data-ttu-id="401c3-154">Usare il provider unpkg.</span><span class="sxs-lookup"><span data-stu-id="401c3-154">Use the unpkg provider.</span></span>
  * <span data-ttu-id="401c3-155">Copiare i file nella destinazione *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="401c3-155">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="401c3-156">Copiare solo i file specificati.</span><span class="sxs-lookup"><span data-stu-id="401c3-156">Copy only the specified files.</span></span>

  <span data-ttu-id="401c3-157">L'output è simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="401c3-157">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="401c3-158">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="401c3-158">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="401c3-159">Nel **Terminale** eseguire il comando seguente per installare LibMan.</span><span class="sxs-lookup"><span data-stu-id="401c3-159">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="401c3-160">Passare alla cartella del progetto, ovvero quella che contiene il file *SignalRChat.csproj*.</span><span class="sxs-lookup"><span data-stu-id="401c3-160">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="401c3-161">Eseguire il comando seguente per ottenere la libreria client di SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="401c3-161">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="401c3-162">I parametri specificano le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="401c3-162">The parameters specify the following options:</span></span>
  * <span data-ttu-id="401c3-163">Usare il provider unpkg.</span><span class="sxs-lookup"><span data-stu-id="401c3-163">Use the unpkg provider.</span></span>
  * <span data-ttu-id="401c3-164">Copiare i file nella destinazione *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="401c3-164">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="401c3-165">Copiare solo i file specificati.</span><span class="sxs-lookup"><span data-stu-id="401c3-165">Copy only the specified files.</span></span>

  <span data-ttu-id="401c3-166">L'output è simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="401c3-166">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="401c3-167">Creare un hub di SignalR</span><span class="sxs-lookup"><span data-stu-id="401c3-167">Create a SignalR hub</span></span>

<span data-ttu-id="401c3-168">Un *hub* è una classe usata come pipeline di alto livello che gestisce le comunicazioni client-server.</span><span class="sxs-lookup"><span data-stu-id="401c3-168">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="401c3-169">Nella cartella del progetto SignalRChat creare una cartella *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="401c3-169">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="401c3-170">Nella cartella *Hubs* creare un file *ChatHub.cs* contenente il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="401c3-170">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="401c3-171">La classe `ChatHub` eredita dalla classe `Hub` di SignalR.</span><span class="sxs-lookup"><span data-stu-id="401c3-171">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="401c3-172">La classe `Hub` gestisce connessioni, gruppi e messaggistica.</span><span class="sxs-lookup"><span data-stu-id="401c3-172">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="401c3-173">Il metodo `SendMessage` può essere chiamato da un client connesso per inviare un messaggio a tutti i client.</span><span class="sxs-lookup"><span data-stu-id="401c3-173">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="401c3-174">Il codice client JavaScript che chiama il metodo è illustrato più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="401c3-174">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="401c3-175">Il codice di SignalR è asincrono allo scopo di offrire la massima scalabilità.</span><span class="sxs-lookup"><span data-stu-id="401c3-175">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="401c3-176">Configurare SignalR</span><span class="sxs-lookup"><span data-stu-id="401c3-176">Configure SignalR</span></span>

<span data-ttu-id="401c3-177">È necessario configurare il server SignalR in modo che passi le richieste SignalR a SignalR.</span><span class="sxs-lookup"><span data-stu-id="401c3-177">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="401c3-178">Aggiungere il codice evidenziato seguente al file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="401c3-178">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="401c3-179">Con queste modifiche SignalR viene aggiunto al sistema di inserimento delle dipendenze di ASP.NET Core e alla pipeline middleware.</span><span class="sxs-lookup"><span data-stu-id="401c3-179">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="401c3-180">Aggiungere il codice del client SignalR</span><span class="sxs-lookup"><span data-stu-id="401c3-180">Add SignalR client code</span></span>

* <span data-ttu-id="401c3-181">Sostituire il codice in *Pages\Index.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="401c3-181">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="401c3-182">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="401c3-182">The preceding code:</span></span>

  * <span data-ttu-id="401c3-183">Crea le caselle di testo per il nome e il messaggio di testo, nonché un pulsante di invio.</span><span class="sxs-lookup"><span data-stu-id="401c3-183">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="401c3-184">Crea un elenco con `id="messagesList"` per visualizzare i messaggi ricevuti dall'hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="401c3-184">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="401c3-185">Include i riferimenti agli script in SignalR e il codice dell'applicazione *chat.js* creato nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="401c3-185">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="401c3-186">Nella cartella *wwwroot/js* creare un file *chat.js* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="401c3-186">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="401c3-187">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="401c3-187">The preceding code:</span></span>

  * <span data-ttu-id="401c3-188">Crea e avvia una connessione.</span><span class="sxs-lookup"><span data-stu-id="401c3-188">Creates and starts a connection.</span></span>
  * <span data-ttu-id="401c3-189">Aggiunge al pulsante di invio un gestore che invia messaggi all'hub.</span><span class="sxs-lookup"><span data-stu-id="401c3-189">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="401c3-190">Aggiunge all'oggetto connessione un gestore che riceve i messaggi dall'hub e li aggiunge all'elenco.</span><span class="sxs-lookup"><span data-stu-id="401c3-190">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="401c3-191">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="401c3-191">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="401c3-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="401c3-192">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="401c3-193">Premere **CTRL+F5** per eseguire l'app senza debug.</span><span class="sxs-lookup"><span data-stu-id="401c3-193">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="401c3-194">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="401c3-194">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="401c3-195">Eseguire il comando seguente nel terminale integrato:</span><span class="sxs-lookup"><span data-stu-id="401c3-195">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="401c3-196">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="401c3-196">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="401c3-197">Nel menu selezionare **Esegui > Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="401c3-197">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="401c3-198">Copiare l'URL dalla barra degli indirizzi, aprire un'altra istanza o scheda del browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="401c3-198">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="401c3-199">Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia messaggio**.</span><span class="sxs-lookup"><span data-stu-id="401c3-199">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="401c3-200">Nome e messaggio vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="401c3-200">The name and message are displayed on both pages instantly.</span></span>

  ![App SignalR di esempio](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="401c3-202">Se l'app non funziona, aprire gli strumenti di sviluppo (F12) del browser e passare alla console.</span><span class="sxs-lookup"><span data-stu-id="401c3-202">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="401c3-203">È possibile che vengano visualizzati errori correlati al codice HTML e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="401c3-203">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="401c3-204">Ad esempio, si supponga di aver inserito *signalr.js* in una cartella diversa da quella indicata nelle istruzioni.</span><span class="sxs-lookup"><span data-stu-id="401c3-204">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="401c3-205">In questo caso il riferimento a tale file non funzionerà e verrà visualizzato un errore 404 nella console.</span><span class="sxs-lookup"><span data-stu-id="401c3-205">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="401c3-206">![Errore di signalr.js non trovato](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="401c3-206">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="401c3-207">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="401c3-207">Next steps</span></span>

<span data-ttu-id="401c3-208">In questa esercitazione si è appreso come:</span><span class="sxs-lookup"><span data-stu-id="401c3-208">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="401c3-209">Creare un progetto di app Web.</span><span class="sxs-lookup"><span data-stu-id="401c3-209">Create a web app project.</span></span>
> * <span data-ttu-id="401c3-210">Aggiungere la libreria client di SignalR.</span><span class="sxs-lookup"><span data-stu-id="401c3-210">Add the SignalR client library.</span></span>
> * <span data-ttu-id="401c3-211">Creare un hub di SignalR.</span><span class="sxs-lookup"><span data-stu-id="401c3-211">Create a SignalR hub.</span></span>
> * <span data-ttu-id="401c3-212">Configurare il progetto per l'uso di SignalR.</span><span class="sxs-lookup"><span data-stu-id="401c3-212">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="401c3-213">Aggiungere il codice che usa l'hub per inviare messaggi a tutti i client connessi da qualsiasi client.</span><span class="sxs-lookup"><span data-stu-id="401c3-213">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="401c3-214">Per altre informazioni su SignalR, vedere l'introduzione:</span><span class="sxs-lookup"><span data-stu-id="401c3-214">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="401c3-215">Introduzione a SignalR di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="401c3-215">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
