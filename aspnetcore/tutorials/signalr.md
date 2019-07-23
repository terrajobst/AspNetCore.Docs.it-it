---
title: Introduzione ad ASP.NET Core SignalR
author: bradygaster
description: In questa esercitazione viene creata un'app di chat che usa ASP.NET Core SignalR.
ms.author: bradyg
ms.custom: mvc
ms.date: 07/08/2019
uid: tutorials/signalr
ms.openlocfilehash: 53d3763a93cc72b6bcf85b64a706500299b3597f
ms.sourcegitcommit: 040aedca220ed24ee1726e6886daf6906f95a028
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/15/2019
ms.locfileid: "67893726"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="3355e-103">Esercitazione: Introduzione ad ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="3355e-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3355e-104">Questa esercitazione illustra le nozioni di base per compilare un'app in tempo reale con SignalR.</span><span class="sxs-lookup"><span data-stu-id="3355e-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="3355e-105">Vengono illustrate le seguenti procedure:</span><span class="sxs-lookup"><span data-stu-id="3355e-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3355e-106">Creare un progetto Web.</span><span class="sxs-lookup"><span data-stu-id="3355e-106">Create a web project.</span></span>
> * <span data-ttu-id="3355e-107">Aggiungere la libreria client di SignalR.</span><span class="sxs-lookup"><span data-stu-id="3355e-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="3355e-108">Creare un hub di SignalR.</span><span class="sxs-lookup"><span data-stu-id="3355e-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="3355e-109">Configurare il progetto per l'uso di SignalR.</span><span class="sxs-lookup"><span data-stu-id="3355e-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="3355e-110">Aggiungere codice che invia messaggi da qualsiasi client a tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="3355e-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="3355e-111">Al termine, si disporrà di un'app di chat funzionante:</span><span class="sxs-lookup"><span data-stu-id="3355e-111">At the end, you'll have a working chat app:</span></span>

![App SignalR di esempio](signalr/_static/3.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="3355e-113">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="3355e-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3355e-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3355e-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3355e-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3355e-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3355e-116">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3355e-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-app-project"></a><span data-ttu-id="3355e-117">Creare un progetto di app Web</span><span class="sxs-lookup"><span data-stu-id="3355e-117">Create a web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3355e-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3355e-118">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="3355e-119">Nel menu selezionare **File > Nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="3355e-119">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="3355e-120">Nella finestra di dialogo **Crea un nuovo progetto** selezionare **Applicazione Web ASP.NET Core**, quindi selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="3355e-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**, and then select **Next**.</span></span>

* <span data-ttu-id="3355e-121">Nella finestra di dialogo **Configura il nuovo progetto**, denominare il progetto *SignalRChat*, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="3355e-121">In the **Configure your new project** dialog, name the project *SignalRChat*, and then select **Create**.</span></span>

* <span data-ttu-id="3355e-122">Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core** verificare che siano selezionati **.NET Core** e **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="3355e-122">In the **Create a new ASP.NET Core Web Application** dialog, select **.NET Core** and **ASP.NET Core 3.0**.</span></span> 

* <span data-ttu-id="3355e-123">Selezionare **Applicazione Web** per creare un progetto che usa Razor Pages, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="3355e-123">Select **Web Application** to create a project that uses Razor Pages, and then select **Create**.</span></span>

  ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/3.x/signalr-new-project-dialog.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3355e-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3355e-125">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="3355e-126">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal) alla cartella in cui verrà creata la nuova cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="3355e-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="3355e-127">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="3355e-127">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3355e-128">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3355e-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="3355e-129">Nel menu selezionare **File > Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="3355e-129">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="3355e-130">Selezionare **.NET Core > App > Applicazione Web** (Non selezionare **Applicazione Web (Model-View-Controller)** ), quindi selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="3355e-130">Select **.NET Core > App > Web Application** (Don't select **Web Application (Model-View-Controller)**), and then select **Next**.</span></span>

* <span data-ttu-id="3355e-131">Assicurarsi che il **framework di destinazione** sia impostato su **.NET Core 3.0**, quindi selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="3355e-131">Make sure the **Target Framework** is set to **.NET Core 3.0**, and then select **Next**.</span></span>

* <span data-ttu-id="3355e-132">Assegnare al progetto il nome *SignalRChat* e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="3355e-132">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="3355e-133">Aggiungere la libreria client di SignalR</span><span class="sxs-lookup"><span data-stu-id="3355e-133">Add the SignalR client library</span></span>

<span data-ttu-id="3355e-134">La libreria server di SignalR è inclusa nel framework condiviso ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="3355e-134">The SignalR server library is included in the ASP.NET Core 3.0 shared framework.</span></span> <span data-ttu-id="3355e-135">La libreria client JavaScript non viene inclusa automaticamente nel progetto.</span><span class="sxs-lookup"><span data-stu-id="3355e-135">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="3355e-136">Per questa esercitazione, usare Gestione librerie (LibMan) per ottenere la libreria client da *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="3355e-136">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="3355e-137">Unpkg è una rete per la distribuzione di contenuti (CDN, Content Delivery Network) che può offrire qualsiasi contenuto disponibile in npm, la gestione pacchetti di Node.js.</span><span class="sxs-lookup"><span data-stu-id="3355e-137">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3355e-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3355e-138">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="3355e-139">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto e selezionare **Aggiungi** > **Libreria lato client**.</span><span class="sxs-lookup"><span data-stu-id="3355e-139">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="3355e-140">Nella finestra di dialogo **Add Client-Side Library** (Aggiungi libreria lato client) selezionare **unpkg** in **Provider**.</span><span class="sxs-lookup"><span data-stu-id="3355e-140">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="3355e-141">Per la **Library**, immettere `@aspnet/signalr@next`.</span><span class="sxs-lookup"><span data-stu-id="3355e-141">For **Library**, enter `@aspnet/signalr@next`.</span></span>
<!-- when 3.0 is released, change @next to @latest -->

* <span data-ttu-id="3355e-142">Selezionare **Choose specific files** (Scegli file specifici), espandere la cartella *dist/browser* e selezionare *signalr.js* e *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="3355e-142">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="3355e-143">Impostare **Percorso di destinazione** su *wwwroot/lib/signalr/* e selezionare **Installa**.</span><span class="sxs-lookup"><span data-stu-id="3355e-143">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Finestra di dialogo Add Client-Side Library (Aggiungi libreria lato client) - selezione della libreria](signalr/_static/3.x/libman1.png)

  <span data-ttu-id="3355e-145">LibMan crea una cartella *wwwroot/lib/signalr* e copia i file selezionati in tale cartella.</span><span class="sxs-lookup"><span data-stu-id="3355e-145">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3355e-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3355e-146">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="3355e-147">Eseguire il comando seguente nel terminale integrato per installare LibMan.</span><span class="sxs-lookup"><span data-stu-id="3355e-147">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="3355e-148">Eseguire il comando seguente per ottenere la libreria client di SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="3355e-148">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="3355e-149">Potrebbe essere necessario attendere alcuni secondi prima di visualizzare l'output.</span><span class="sxs-lookup"><span data-stu-id="3355e-149">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="3355e-150">I parametri specificano le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="3355e-150">The parameters specify the following options:</span></span>
  * <span data-ttu-id="3355e-151">Usare il provider unpkg.</span><span class="sxs-lookup"><span data-stu-id="3355e-151">Use the unpkg provider.</span></span>
  * <span data-ttu-id="3355e-152">Copiare i file nella destinazione *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="3355e-152">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="3355e-153">Copiare solo i file specificati.</span><span class="sxs-lookup"><span data-stu-id="3355e-153">Copy only the specified files.</span></span>

  <span data-ttu-id="3355e-154">L'output è simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="3355e-154">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3355e-155">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3355e-155">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="3355e-156">Nel **Terminale** eseguire il comando seguente per installare LibMan.</span><span class="sxs-lookup"><span data-stu-id="3355e-156">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="3355e-157">Passare alla cartella del progetto, ovvero quella che contiene il file *SignalRChat.csproj*.</span><span class="sxs-lookup"><span data-stu-id="3355e-157">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="3355e-158">Eseguire il comando seguente per ottenere la libreria client di SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="3355e-158">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr@next -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="3355e-159">I parametri specificano le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="3355e-159">The parameters specify the following options:</span></span>
  * <span data-ttu-id="3355e-160">Usare il provider unpkg.</span><span class="sxs-lookup"><span data-stu-id="3355e-160">Use the unpkg provider.</span></span>
  * <span data-ttu-id="3355e-161">Copiare i file nella destinazione *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="3355e-161">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="3355e-162">Copiare solo i file specificati.</span><span class="sxs-lookup"><span data-stu-id="3355e-162">Copy only the specified files.</span></span>

  <span data-ttu-id="3355e-163">L'output è simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="3355e-163">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@next" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="3355e-164">Creare un hub di SignalR</span><span class="sxs-lookup"><span data-stu-id="3355e-164">Create a SignalR hub</span></span>

<span data-ttu-id="3355e-165">Un *hub* è una classe usata come pipeline di alto livello che gestisce le comunicazioni client-server.</span><span class="sxs-lookup"><span data-stu-id="3355e-165">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="3355e-166">Nella cartella del progetto SignalRChat creare una cartella *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="3355e-166">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="3355e-167">Nella cartella *Hubs* creare un file *ChatHub.cs* contenente il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="3355e-167">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/ChatHub.cs)]

  <span data-ttu-id="3355e-168">La classe `ChatHub` eredita dalla classe `Hub` di SignalR.</span><span class="sxs-lookup"><span data-stu-id="3355e-168">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="3355e-169">La classe `Hub` gestisce connessioni, gruppi e messaggistica.</span><span class="sxs-lookup"><span data-stu-id="3355e-169">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="3355e-170">Il metodo `SendMessage` può essere chiamato da un client connesso per inviare un messaggio a tutti i client.</span><span class="sxs-lookup"><span data-stu-id="3355e-170">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="3355e-171">Il codice client JavaScript che chiama il metodo è illustrato più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="3355e-171">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="3355e-172">Il codice di SignalR è asincrono allo scopo di offrire la massima scalabilità.</span><span class="sxs-lookup"><span data-stu-id="3355e-172">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="3355e-173">Configurare SignalR</span><span class="sxs-lookup"><span data-stu-id="3355e-173">Configure SignalR</span></span>

<span data-ttu-id="3355e-174">È necessario configurare il server SignalR in modo che passi le richieste SignalR a SignalR.</span><span class="sxs-lookup"><span data-stu-id="3355e-174">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="3355e-175">Aggiungere il codice evidenziato seguente al file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="3355e-175">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/3.x/Startup.cs?highlight=6,30,58)]

  <span data-ttu-id="3355e-176">Con queste modifiche SignalR viene aggiunto ai sistemi di inserimento delle dipendenze e di routing di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3355e-176">These changes add SignalR to the ASP.NET Core dependency injection and routing systems.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="3355e-177">Aggiungere il codice del client SignalR</span><span class="sxs-lookup"><span data-stu-id="3355e-177">Add SignalR client code</span></span>

* <span data-ttu-id="3355e-178">Sostituire il codice in *Pages\Index.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="3355e-178">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/3.x/Index.cshtml)]

  <span data-ttu-id="3355e-179">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="3355e-179">The preceding code:</span></span>

  * <span data-ttu-id="3355e-180">Crea le caselle di testo per il nome e il messaggio di testo, nonché un pulsante di invio.</span><span class="sxs-lookup"><span data-stu-id="3355e-180">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="3355e-181">Crea un elenco con `id="messagesList"` per visualizzare i messaggi ricevuti dall'hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="3355e-181">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="3355e-182">Include i riferimenti agli script in SignalR e il codice dell'applicazione *chat.js* creato nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="3355e-182">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="3355e-183">Nella cartella *wwwroot/js* creare un file *chat.js* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="3355e-183">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample-snapshot/3.x/chat.js)]

  <span data-ttu-id="3355e-184">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="3355e-184">The preceding code:</span></span>

  * <span data-ttu-id="3355e-185">Crea e avvia una connessione.</span><span class="sxs-lookup"><span data-stu-id="3355e-185">Creates and starts a connection.</span></span>
  * <span data-ttu-id="3355e-186">Aggiunge al pulsante di invio un gestore che invia messaggi all'hub.</span><span class="sxs-lookup"><span data-stu-id="3355e-186">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="3355e-187">Aggiunge all'oggetto connessione un gestore che riceve i messaggi dall'hub e li aggiunge all'elenco.</span><span class="sxs-lookup"><span data-stu-id="3355e-187">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="3355e-188">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="3355e-188">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3355e-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3355e-189">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3355e-190">Premere **CTRL+F5** per eseguire l'app senza debug.</span><span class="sxs-lookup"><span data-stu-id="3355e-190">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3355e-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3355e-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="3355e-192">Eseguire il comando seguente nel terminale integrato:</span><span class="sxs-lookup"><span data-stu-id="3355e-192">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3355e-193">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3355e-193">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="3355e-194">Nel menu selezionare **Esegui > Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="3355e-194">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="3355e-195">Copiare l'URL dalla barra degli indirizzi, aprire un'altra istanza o scheda del browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="3355e-195">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="3355e-196">Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia messaggio**.</span><span class="sxs-lookup"><span data-stu-id="3355e-196">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="3355e-197">Nome e messaggio vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="3355e-197">The name and message are displayed on both pages instantly.</span></span>

  ![App SignalR di esempio](signalr/_static/3.x/signalr-get-started-finished.png)

> [!TIP]
> * <span data-ttu-id="3355e-199">Se l'app non funziona, aprire gli strumenti di sviluppo (F12) del browser e passare alla console.</span><span class="sxs-lookup"><span data-stu-id="3355e-199">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="3355e-200">È possibile che vengano visualizzati errori correlati al codice HTML e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3355e-200">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="3355e-201">Ad esempio, si supponga di aver inserito *signalr.js* in una cartella diversa da quella indicata nelle istruzioni.</span><span class="sxs-lookup"><span data-stu-id="3355e-201">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="3355e-202">In questo caso il riferimento a tale file non funzionerà e verrà visualizzato un errore 404 nella console.</span><span class="sxs-lookup"><span data-stu-id="3355e-202">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
>   <span data-ttu-id="3355e-203">![Errore di signalr.js non trovato](signalr/_static/3.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="3355e-203">![signalr.js not found error](signalr/_static/3.x/f12-console.png)</span></span>
> * <span data-ttu-id="3355e-204">Se viene visualizzato l'errore ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY in Chrome o NS_ERROR_NET_INADEQUATE_SECURITY in Firefox, eseguire questi comandi per aggiornare il proprio certificato di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="3355e-204">If you get the error ERR_SPDY_INADEQUATE_TRANSPORT_SECURITY in Chrome or NS_ERROR_NET_INADEQUATE_SECURITY in Firefox, run these commands to update your development certificate:</span></span>
>   ```
>   dotnet dev-certs https --clean
>   dotnet dev-certs https --trust
>   ```

## <a name="next-steps"></a><span data-ttu-id="3355e-205">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="3355e-205">Next steps</span></span>

<span data-ttu-id="3355e-206">Per altre informazioni su SignalR, vedere l'introduzione:</span><span class="sxs-lookup"><span data-stu-id="3355e-206">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="3355e-207">Introduzione a SignalR di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3355e-207">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="3355e-208">Questa esercitazione illustra le nozioni di base per compilare un'app in tempo reale con SignalR.</span><span class="sxs-lookup"><span data-stu-id="3355e-208">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="3355e-209">Vengono illustrate le seguenti procedure:</span><span class="sxs-lookup"><span data-stu-id="3355e-209">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3355e-210">Creare un progetto Web.</span><span class="sxs-lookup"><span data-stu-id="3355e-210">Create a web project.</span></span>
> * <span data-ttu-id="3355e-211">Aggiungere la libreria client di SignalR.</span><span class="sxs-lookup"><span data-stu-id="3355e-211">Add the SignalR client library.</span></span>
> * <span data-ttu-id="3355e-212">Creare un hub di SignalR.</span><span class="sxs-lookup"><span data-stu-id="3355e-212">Create a SignalR hub.</span></span>
> * <span data-ttu-id="3355e-213">Configurare il progetto per l'uso di SignalR.</span><span class="sxs-lookup"><span data-stu-id="3355e-213">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="3355e-214">Aggiungere codice che invia messaggi da qualsiasi client a tutti i client connessi.</span><span class="sxs-lookup"><span data-stu-id="3355e-214">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="3355e-215">Al termine, si disporrà di un'app di chat funzionante:</span><span class="sxs-lookup"><span data-stu-id="3355e-215">At the end, you'll have a working chat app:</span></span>

![App SignalR di esempio](signalr/_static/2.x/signalr-get-started-finished.png)

## <a name="prerequisites"></a><span data-ttu-id="3355e-217">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="3355e-217">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3355e-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3355e-218">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2017-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3355e-219">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3355e-219">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3355e-220">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3355e-220">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="3355e-221">Creare un progetto Web</span><span class="sxs-lookup"><span data-stu-id="3355e-221">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3355e-222">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3355e-222">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="3355e-223">Nel menu selezionare **File > Nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="3355e-223">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="3355e-224">Nella finestra di dialogo **Nuovo progetto** selezionare **Installate > Visual C# > Web > Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="3355e-224">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="3355e-225">Denominare il progetto *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="3355e-225">Name the project *SignalRChat*.</span></span>

  ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/2.x/signalr-new-project-dialog.png)

* <span data-ttu-id="3355e-227">Selezionare **Applicazione Web** per creare un progetto che usa Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="3355e-227">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="3355e-228">Selezionare un framework di destinazione di **.NET Core**, selezionare **ASP.NET Core 2.2** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="3355e-228">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>

  ![Finestra di dialogo Nuovo progetto di Visual Studio](signalr/_static/2.x/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3355e-230">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3355e-230">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="3355e-231">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal) alla cartella in cui verrà creata la nuova cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="3355e-231">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="3355e-232">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="3355e-232">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3355e-233">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3355e-233">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="3355e-234">Nel menu selezionare **File > Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="3355e-234">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="3355e-235">Selezionare **.NET Core > App > App Web ASP.NET Core** (non selezionare **App Web ASP.NET Core (MVC)** ).</span><span class="sxs-lookup"><span data-stu-id="3355e-235">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="3355e-236">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="3355e-236">Select **Next**.</span></span>

* <span data-ttu-id="3355e-237">Assegnare al progetto il nome *SignalRChat* e quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="3355e-237">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="3355e-238">Aggiungere la libreria client di SignalR</span><span class="sxs-lookup"><span data-stu-id="3355e-238">Add the SignalR client library</span></span>

<span data-ttu-id="3355e-239">La libreria server di SignalR è inclusa nel metapacchetto `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="3355e-239">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="3355e-240">La libreria client JavaScript non viene inclusa automaticamente nel progetto.</span><span class="sxs-lookup"><span data-stu-id="3355e-240">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="3355e-241">Per questa esercitazione, usare Gestione librerie (LibMan) per ottenere la libreria client da *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="3355e-241">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="3355e-242">Unpkg è una rete per la distribuzione di contenuti (CDN, Content Delivery Network) che può offrire qualsiasi contenuto disponibile in npm, la gestione pacchetti di Node.js.</span><span class="sxs-lookup"><span data-stu-id="3355e-242">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3355e-243">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3355e-243">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="3355e-244">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto e selezionare **Aggiungi** > **Libreria lato client**.</span><span class="sxs-lookup"><span data-stu-id="3355e-244">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="3355e-245">Nella finestra di dialogo **Add Client-Side Library** (Aggiungi libreria lato client) selezionare **unpkg** in **Provider**.</span><span class="sxs-lookup"><span data-stu-id="3355e-245">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span>

* <span data-ttu-id="3355e-246">In **Libreria** immettere `@aspnet/signalr@1` e selezionare la versione più recente che non sia di anteprima.</span><span class="sxs-lookup"><span data-stu-id="3355e-246">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Finestra di dialogo Add Client-Side Library (Aggiungi libreria lato client) - selezione della libreria](signalr/_static/2.x/libman1.png)

* <span data-ttu-id="3355e-248">Selezionare **Choose specific files** (Scegli file specifici), espandere la cartella *dist/browser* e selezionare *signalr.js* e *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="3355e-248">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="3355e-249">Impostare **Percorso di destinazione** su *wwwroot/lib/signalr/* e selezionare **Installa**.</span><span class="sxs-lookup"><span data-stu-id="3355e-249">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Finestra di dialogo Add Client-Side Library (Aggiungi libreria lato client) - selezione di file e destinazione](signalr/_static/2.x/libman2.png)

  <span data-ttu-id="3355e-251">LibMan crea una cartella *wwwroot/lib/signalr* e copia i file selezionati in tale cartella.</span><span class="sxs-lookup"><span data-stu-id="3355e-251">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3355e-252">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3355e-252">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="3355e-253">Eseguire il comando seguente nel terminale integrato per installare LibMan.</span><span class="sxs-lookup"><span data-stu-id="3355e-253">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="3355e-254">Eseguire il comando seguente per ottenere la libreria client di SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="3355e-254">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="3355e-255">Potrebbe essere necessario attendere alcuni secondi prima di visualizzare l'output.</span><span class="sxs-lookup"><span data-stu-id="3355e-255">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="3355e-256">I parametri specificano le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="3355e-256">The parameters specify the following options:</span></span>
  * <span data-ttu-id="3355e-257">Usare il provider unpkg.</span><span class="sxs-lookup"><span data-stu-id="3355e-257">Use the unpkg provider.</span></span>
  * <span data-ttu-id="3355e-258">Copiare i file nella destinazione *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="3355e-258">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="3355e-259">Copiare solo i file specificati.</span><span class="sxs-lookup"><span data-stu-id="3355e-259">Copy only the specified files.</span></span>

  <span data-ttu-id="3355e-260">L'output è simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="3355e-260">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3355e-261">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3355e-261">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="3355e-262">Nel **Terminale** eseguire il comando seguente per installare LibMan.</span><span class="sxs-lookup"><span data-stu-id="3355e-262">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="3355e-263">Passare alla cartella del progetto, ovvero quella che contiene il file *SignalRChat.csproj*.</span><span class="sxs-lookup"><span data-stu-id="3355e-263">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="3355e-264">Eseguire il comando seguente per ottenere la libreria client di SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="3355e-264">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="3355e-265">I parametri specificano le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="3355e-265">The parameters specify the following options:</span></span>
  * <span data-ttu-id="3355e-266">Usare il provider unpkg.</span><span class="sxs-lookup"><span data-stu-id="3355e-266">Use the unpkg provider.</span></span>
  * <span data-ttu-id="3355e-267">Copiare i file nella destinazione *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="3355e-267">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="3355e-268">Copiare solo i file specificati.</span><span class="sxs-lookup"><span data-stu-id="3355e-268">Copy only the specified files.</span></span>

  <span data-ttu-id="3355e-269">L'output è simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="3355e-269">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="3355e-270">Creare un hub di SignalR</span><span class="sxs-lookup"><span data-stu-id="3355e-270">Create a SignalR hub</span></span>

<span data-ttu-id="3355e-271">Un *hub* è una classe usata come pipeline di alto livello che gestisce le comunicazioni client-server.</span><span class="sxs-lookup"><span data-stu-id="3355e-271">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="3355e-272">Nella cartella del progetto SignalRChat creare una cartella *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="3355e-272">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="3355e-273">Nella cartella *Hubs* creare un file *ChatHub.cs* contenente il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="3355e-273">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/ChatHub.cs)]

  <span data-ttu-id="3355e-274">La classe `ChatHub` eredita dalla classe `Hub` di SignalR.</span><span class="sxs-lookup"><span data-stu-id="3355e-274">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="3355e-275">La classe `Hub` gestisce connessioni, gruppi e messaggistica.</span><span class="sxs-lookup"><span data-stu-id="3355e-275">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="3355e-276">Il metodo `SendMessage` può essere chiamato da un client connesso per inviare un messaggio a tutti i client.</span><span class="sxs-lookup"><span data-stu-id="3355e-276">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="3355e-277">Il codice client JavaScript che chiama il metodo è illustrato più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="3355e-277">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="3355e-278">Il codice di SignalR è asincrono allo scopo di offrire la massima scalabilità.</span><span class="sxs-lookup"><span data-stu-id="3355e-278">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="3355e-279">Configurare SignalR</span><span class="sxs-lookup"><span data-stu-id="3355e-279">Configure SignalR</span></span>

<span data-ttu-id="3355e-280">È necessario configurare il server SignalR in modo che passi le richieste SignalR a SignalR.</span><span class="sxs-lookup"><span data-stu-id="3355e-280">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="3355e-281">Aggiungere il codice evidenziato seguente al file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="3355e-281">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample-snapshot/2.x/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="3355e-282">Con queste modifiche SignalR viene aggiunto al sistema di inserimento delle dipendenze di ASP.NET Core e alla pipeline middleware.</span><span class="sxs-lookup"><span data-stu-id="3355e-282">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="3355e-283">Aggiungere il codice del client SignalR</span><span class="sxs-lookup"><span data-stu-id="3355e-283">Add SignalR client code</span></span>

* <span data-ttu-id="3355e-284">Sostituire il codice in *Pages\Index.cshtml* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="3355e-284">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample-snapshot/2.x/Index.cshtml)]

  <span data-ttu-id="3355e-285">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="3355e-285">The preceding code:</span></span>

  * <span data-ttu-id="3355e-286">Crea le caselle di testo per il nome e il messaggio di testo, nonché un pulsante di invio.</span><span class="sxs-lookup"><span data-stu-id="3355e-286">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="3355e-287">Crea un elenco con `id="messagesList"` per visualizzare i messaggi ricevuti dall'hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="3355e-287">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="3355e-288">Include i riferimenti agli script in SignalR e il codice dell'applicazione *chat.js* creato nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="3355e-288">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="3355e-289">Nella cartella *wwwroot/js* creare un file *chat.js* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="3355e-289">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample-snapshot/2.x/chat.js)]

  <span data-ttu-id="3355e-290">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="3355e-290">The preceding code:</span></span>

  * <span data-ttu-id="3355e-291">Crea e avvia una connessione.</span><span class="sxs-lookup"><span data-stu-id="3355e-291">Creates and starts a connection.</span></span>
  * <span data-ttu-id="3355e-292">Aggiunge al pulsante di invio un gestore che invia messaggi all'hub.</span><span class="sxs-lookup"><span data-stu-id="3355e-292">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="3355e-293">Aggiunge all'oggetto connessione un gestore che riceve i messaggi dall'hub e li aggiunge all'elenco.</span><span class="sxs-lookup"><span data-stu-id="3355e-293">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="3355e-294">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="3355e-294">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3355e-295">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3355e-295">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3355e-296">Premere **CTRL+F5** per eseguire l'app senza debug.</span><span class="sxs-lookup"><span data-stu-id="3355e-296">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3355e-297">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3355e-297">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="3355e-298">Eseguire il comando seguente nel terminale integrato:</span><span class="sxs-lookup"><span data-stu-id="3355e-298">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3355e-299">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3355e-299">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="3355e-300">Nel menu selezionare **Esegui > Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="3355e-300">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="3355e-301">Copiare l'URL dalla barra degli indirizzi, aprire un'altra istanza o scheda del browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="3355e-301">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="3355e-302">Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia messaggio**.</span><span class="sxs-lookup"><span data-stu-id="3355e-302">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="3355e-303">Nome e messaggio vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="3355e-303">The name and message are displayed on both pages instantly.</span></span>

  ![App SignalR di esempio](signalr/_static/2.x/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="3355e-305">Se l'app non funziona, aprire gli strumenti di sviluppo (F12) del browser e passare alla console.</span><span class="sxs-lookup"><span data-stu-id="3355e-305">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="3355e-306">È possibile che vengano visualizzati errori correlati al codice HTML e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3355e-306">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="3355e-307">Ad esempio, si supponga di aver inserito *signalr.js* in una cartella diversa da quella indicata nelle istruzioni.</span><span class="sxs-lookup"><span data-stu-id="3355e-307">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="3355e-308">In questo caso il riferimento a tale file non funzionerà e verrà visualizzato un errore 404 nella console.</span><span class="sxs-lookup"><span data-stu-id="3355e-308">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="3355e-309">![Errore di signalr.js non trovato](signalr/_static/2.x/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="3355e-309">![signalr.js not found error](signalr/_static/2.x/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="3355e-310">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="3355e-310">Next steps</span></span>

<span data-ttu-id="3355e-311">In questa esercitazione si è appreso come:</span><span class="sxs-lookup"><span data-stu-id="3355e-311">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3355e-312">Creare un progetto di app Web.</span><span class="sxs-lookup"><span data-stu-id="3355e-312">Create a web app project.</span></span>
> * <span data-ttu-id="3355e-313">Aggiungere la libreria client di SignalR.</span><span class="sxs-lookup"><span data-stu-id="3355e-313">Add the SignalR client library.</span></span>
> * <span data-ttu-id="3355e-314">Creare un hub di SignalR.</span><span class="sxs-lookup"><span data-stu-id="3355e-314">Create a SignalR hub.</span></span>
> * <span data-ttu-id="3355e-315">Configurare il progetto per l'uso di SignalR.</span><span class="sxs-lookup"><span data-stu-id="3355e-315">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="3355e-316">Aggiungere il codice che usa l'hub per inviare messaggi a tutti i client connessi da qualsiasi client.</span><span class="sxs-lookup"><span data-stu-id="3355e-316">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="3355e-317">Per altre informazioni su SignalR, vedere l'introduzione:</span><span class="sxs-lookup"><span data-stu-id="3355e-317">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="3355e-318">Introduzione a SignalR di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3355e-318">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)

::: moniker-end
