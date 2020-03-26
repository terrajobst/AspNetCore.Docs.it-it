---
title: Usare ASP.NET Core SignalR con Blazor webassembly
author: guardrex
description: Creare un'app di chat che usa ASP.NET Core SignalR con Blazor webassembly.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: tutorials/signalr-blazor-webassembly
ms.openlocfilehash: c4843dc282e1978b39738e206ecc79ded87fcff9
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306576"
---
# <a name="use-aspnet-core-signalr-with-blazor-webassembly"></a><span data-ttu-id="f3f9d-103">Usare ASP.NET Core SignalR con il webassembly Blazer</span><span class="sxs-lookup"><span data-stu-id="f3f9d-103">Use ASP.NET Core SignalR with Blazor WebAssembly</span></span>

<span data-ttu-id="f3f9d-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f3f9d-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="f3f9d-105">Questa esercitazione illustra le nozioni di base per la creazione di un'app in tempo reale usando SignalR con il webassembly blazer.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-105">This tutorial teaches the basics of building a real-time app using SignalR with Blazor WebAssembly.</span></span> <span data-ttu-id="f3f9d-106">Si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="f3f9d-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f3f9d-107">Creare un progetto di app ospitata webassembly Blazer</span><span class="sxs-lookup"><span data-stu-id="f3f9d-107">Create a Blazor WebAssembly Hosted app project</span></span>
> * <span data-ttu-id="f3f9d-108">Aggiungere la libreria client di SignalR</span><span class="sxs-lookup"><span data-stu-id="f3f9d-108">Add the SignalR client library</span></span>
> * <span data-ttu-id="f3f9d-109">Aggiungere un hub SignalR</span><span class="sxs-lookup"><span data-stu-id="f3f9d-109">Add a SignalR hub</span></span>
> * <span data-ttu-id="f3f9d-110">Aggiungere i servizi SignalR e un endpoint per l'hub SignalR</span><span class="sxs-lookup"><span data-stu-id="f3f9d-110">Add SignalR services and an endpoint for the SignalR hub</span></span>
> * <span data-ttu-id="f3f9d-111">Aggiungere il codice del componente Razor per la chat</span><span class="sxs-lookup"><span data-stu-id="f3f9d-111">Add Razor component code for chat</span></span>

<span data-ttu-id="f3f9d-112">Al termine di questa esercitazione, si disporrà di un'app di chat funzionante.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-112">At the end of this tutorial, you'll have a working chat app.</span></span>

<span data-ttu-id="f3f9d-113">[Visualizzare o scaricare il codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f3f9d-113">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-blazor-webassembly/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f3f9d-114">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="f3f9d-114">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="f3f9d-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f3f9d-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="f3f9d-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f3f9d-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="f3f9d-117">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f3f9d-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

# <a name="net-core-cli"></a>[<span data-ttu-id="f3f9d-118">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="f3f9d-118">.NET Core CLI</span></span>](#tab/netcore-cli/)

[!INCLUDE[](~/includes/3.1-SDK.md)]

---

## <a name="create-a-hosted-blazor-webassembly-app-project"></a><span data-ttu-id="f3f9d-119">Creare un progetto di app webassembly in hosting Blazer</span><span class="sxs-lookup"><span data-stu-id="f3f9d-119">Create a hosted Blazor WebAssembly app project</span></span>

<span data-ttu-id="f3f9d-120">Quando non si usa Visual Studio versione 16,6 Preview 2 o versioni successive, installare il modello [Webassembly Blazer](xref:blazor/hosting-models#blazor-webassembly) .</span><span class="sxs-lookup"><span data-stu-id="f3f9d-120">When not using Visual Studio version 16.6 Preview 2 or later, install the [Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) template.</span></span> <span data-ttu-id="f3f9d-121">Il pacchetto [Microsoft. AspNetCore. Components. webassembly. templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/) include una versione di anteprima mentre Webassembly blazer è in anteprima.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-121">The [Microsoft.AspNetCore.Components.WebAssembly.Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/) package has a preview version while Blazor WebAssembly is in preview.</span></span> <span data-ttu-id="f3f9d-122">In una shell dei comandi eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="f3f9d-122">In a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
```

<span data-ttu-id="f3f9d-123">Seguire le istruzioni per la scelta degli strumenti:</span><span class="sxs-lookup"><span data-stu-id="f3f9d-123">Follow the guidance for your choice of tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="f3f9d-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f3f9d-124">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="f3f9d-125">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-125">Create a new project.</span></span>

1. <span data-ttu-id="f3f9d-126">Selezionare **app Blazer** e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-126">Select **Blazor App** and select **Next**.</span></span>

1. <span data-ttu-id="f3f9d-127">Digitare "BlazorSignalRApp" nel campo **nome progetto** .</span><span class="sxs-lookup"><span data-stu-id="f3f9d-127">Type "BlazorSignalRApp" in the **Project name** field.</span></span> <span data-ttu-id="f3f9d-128">Confermare che la voce relativa al **percorso** sia corretta o specificare un percorso per il progetto.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-128">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="f3f9d-129">Selezionare **Create** (Crea).</span><span class="sxs-lookup"><span data-stu-id="f3f9d-129">Select **Create**.</span></span>

1. <span data-ttu-id="f3f9d-130">Scegliere il modello **app Webassembly Blazer** .</span><span class="sxs-lookup"><span data-stu-id="f3f9d-130">Choose the **Blazor WebAssembly App** template.</span></span>

1. <span data-ttu-id="f3f9d-131">In **Avanzate**selezionare la casella di controllo **ASP.NET Core Hosted** .</span><span class="sxs-lookup"><span data-stu-id="f3f9d-131">Under **Advanced**, select the **ASP.NET Core hosted** check box.</span></span>

1. <span data-ttu-id="f3f9d-132">Selezionare **Create** (Crea).</span><span class="sxs-lookup"><span data-stu-id="f3f9d-132">Select **Create**.</span></span>

> [!NOTE]
> <span data-ttu-id="f3f9d-133">Se è stato eseguito l'aggiornamento o l'installazione di una nuova versione di Visual Studio e il modello webassembly Blazer non viene visualizzato nell'interfaccia utente di Visual Studio, reinstallare il modello usando il comando `dotnet new` illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-133">If you upgraded or installed a new version of Visual Studio and the Blazor WebAssembly template doesn't appear in the VS UI, reinstall the template using the `dotnet new` command shown previously.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="f3f9d-134">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f3f9d-134">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="f3f9d-135">In una shell dei comandi eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="f3f9d-135">In a command shell, execute the following command:</span></span>

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. <span data-ttu-id="f3f9d-136">In Visual Studio Code aprire la cartella del progetto dell'app.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-136">In Visual Studio Code, open the app's project folder.</span></span>

1. <span data-ttu-id="f3f9d-137">Quando viene visualizzata la finestra di dialogo per aggiungere asset per compilare ed eseguire il debug dell'app, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-137">When the dialog appears to add assets to build and debug the app, select **Yes**.</span></span> <span data-ttu-id="f3f9d-138">Visual Studio Code aggiunge automaticamente la cartella *. VSCODE* con i file *Launch. JSON* e *Tasks. JSON* generati.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-138">Visual Studio Code automatically adds the *.vscode* folder with generated *launch.json* and *tasks.json* files.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="f3f9d-139">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f3f9d-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="f3f9d-140">In una shell dei comandi eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="f3f9d-140">In a command shell, execute the following command:</span></span>

   ```dotnetcli
   dotnet new blazorwasm --hosted --output BlazorSignalRApp
   ```

1. <span data-ttu-id="f3f9d-141">In Visual Studio per Mac aprire il progetto passando alla cartella del progetto e aprendo il file di soluzione del progetto ( *. sln*).</span><span class="sxs-lookup"><span data-stu-id="f3f9d-141">In Visual Studio for Mac, open the project by navigating to the project folder and opening the project's solution file (*.sln*).</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="f3f9d-142">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="f3f9d-142">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="f3f9d-143">In una shell dei comandi eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="f3f9d-143">In a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new blazorwasm --hosted --output BlazorSignalRApp
```

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="f3f9d-144">Aggiungere la libreria client di SignalR</span><span class="sxs-lookup"><span data-stu-id="f3f9d-144">Add the SignalR client library</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="f3f9d-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f3f9d-145">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="f3f9d-146">In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto **BlazorSignalRApp. client** e scegliere **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-146">In **Solution Explorer**, right-click the **BlazorSignalRApp.Client** project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="f3f9d-147">Nella finestra di dialogo **Gestisci pacchetti NuGet** verificare che l' **origine del pacchetto** sia impostata su *NuGet.org*.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-147">In the **Manage NuGet Packages** dialog, confirm that the **Package source** is set to *nuget.org*.</span></span>

1. <span data-ttu-id="f3f9d-148">Con **Sfoglia** selezionato, digitare "Microsoft. AspNetCore. SignalR. client" nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-148">With **Browse** selected, type "Microsoft.AspNetCore.SignalR.Client" in the search box.</span></span>

1. <span data-ttu-id="f3f9d-149">Nei risultati della ricerca selezionare il pacchetto di `Microsoft.AspNetCore.SignalR.Client` e selezionare **Installa**.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-149">In the search results, select the `Microsoft.AspNetCore.SignalR.Client` package and select **Install**.</span></span>

1. <span data-ttu-id="f3f9d-150">Se viene visualizzata la finestra di dialogo **Anteprima modifiche** , selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-150">If the **Preview Changes** dialog appears, select **OK**.</span></span>

1. <span data-ttu-id="f3f9d-151">Se viene visualizzata la finestra di dialogo **accettazione della licenza** , selezionare Accetto se **si accettano le** condizioni di licenza.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-151">If the **License Acceptance** dialog appears, select **I Accept** if you agree with the license terms.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="f3f9d-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f3f9d-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

<span data-ttu-id="f3f9d-153">Nel **terminale integrato** (**visualizzare** > **terminale** dalla barra degli strumenti) eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f3f9d-153">In the **Integrated Terminal** (**View** > **Terminal** from the toolbar), execute the following commands:</span></span>

```dotnetcli
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="f3f9d-154">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f3f9d-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="f3f9d-155">Nella barra laterale **soluzione** fare clic con il pulsante destro del mouse sul progetto **BlazorSignalRApp. client** e scegliere **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-155">In the **Solution** sidebar, right-click the **BlazorSignalRApp.Client** project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="f3f9d-156">Nella finestra di dialogo **Gestisci pacchetti NuGet** verificare che l'elenco a discesa origine sia impostato su *NuGet.org*.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-156">In the **Manage NuGet Packages** dialog, confirm that the source drop-down is set to *nuget.org*.</span></span>

1. <span data-ttu-id="f3f9d-157">Con **Sfoglia** selezionato, digitare "Microsoft. AspNetCore. SignalR. client" nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-157">With **Browse** selected, type "Microsoft.AspNetCore.SignalR.Client" in the search box.</span></span>

1. <span data-ttu-id="f3f9d-158">Nei risultati della ricerca selezionare la casella di controllo accanto al pacchetto di `Microsoft.AspNetCore.SignalR.Client` e selezionare **Aggiungi pacchetto**.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-158">In the search results, select the check box next to the `Microsoft.AspNetCore.SignalR.Client` package and select **Add Package**.</span></span>

1. <span data-ttu-id="f3f9d-159">Se viene visualizzata la finestra di dialogo **accettazione della licenza** , selezionare **Accetto** se si accettano le condizioni di licenza.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-159">If the **License Acceptance** dialog appears, select **Accept** if you agree with the license terms.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="f3f9d-160">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="f3f9d-160">.NET Core CLI</span></span>](#tab/netcore-cli/)

<span data-ttu-id="f3f9d-161">In una shell dei comandi eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f3f9d-161">In a command shell, execute the following commands:</span></span>

```dotnetcli
cd BlazorSignalRApp
dotnet add Client package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="add-a-signalr-hub"></a><span data-ttu-id="f3f9d-162">Aggiungere un hub SignalR</span><span class="sxs-lookup"><span data-stu-id="f3f9d-162">Add a SignalR hub</span></span>

<span data-ttu-id="f3f9d-163">Nel progetto **BlazorSignalRApp. Server** creare una cartella *Hub* (plurale) e aggiungere la classe `ChatHub` seguente (*Hub/ChatHub. cs*):</span><span class="sxs-lookup"><span data-stu-id="f3f9d-163">In the **BlazorSignalRApp.Server** project, create a *Hubs* (plural) folder and add the following `ChatHub` class (*Hubs/ChatHub.cs*):</span></span>

[!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Hubs/ChatHub.cs)]

## <a name="add-signalr-services-and-an-endpoint-for-the-signalr-hub"></a><span data-ttu-id="f3f9d-164">Aggiungere i servizi SignalR e un endpoint per l'hub SignalR</span><span class="sxs-lookup"><span data-stu-id="f3f9d-164">Add SignalR services and an endpoint for the SignalR hub</span></span>

1. <span data-ttu-id="f3f9d-165">Nel progetto **BlazorSignalRApp. Server** aprire il file *Startup.cs* .</span><span class="sxs-lookup"><span data-stu-id="f3f9d-165">In the **BlazorSignalRApp.Server** project, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="f3f9d-166">Aggiungere lo spazio dei nomi per la classe `ChatHub` all'inizio del file:</span><span class="sxs-lookup"><span data-stu-id="f3f9d-166">Add the namespace for the `ChatHub` class to the top of the file:</span></span>

   ```csharp
   using BlazorSignalRApp.Server.Hubs;
   ```

1. <span data-ttu-id="f3f9d-167">Aggiungere i servizi SignalR a `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f3f9d-167">Add the SignalR services to `Startup.ConfigureServices`:</span></span>

   ```csharp
   services.AddSignalR();
   ```

1. <span data-ttu-id="f3f9d-168">In `Startup.Configure` tra gli endpoint per la route del controller predefinita e il fallback sul lato client, aggiungere un endpoint per l'hub:</span><span class="sxs-lookup"><span data-stu-id="f3f9d-168">In `Startup.Configure` between the endpoints for the default controller route and the client-side fallback, add an endpoint for the hub:</span></span>

   [!code-csharp[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Server/Startup.cs?name=snippet&highlight=4)]

## <a name="add-razor-component-code-for-chat"></a><span data-ttu-id="f3f9d-169">Aggiungere il codice del componente Razor per la chat</span><span class="sxs-lookup"><span data-stu-id="f3f9d-169">Add Razor component code for chat</span></span>

1. <span data-ttu-id="f3f9d-170">Nel progetto **BlazorSignalRApp. client** aprire il file *pages/index. Razor* .</span><span class="sxs-lookup"><span data-stu-id="f3f9d-170">In the **BlazorSignalRApp.Client** project, open the *Pages/Index.razor* file.</span></span>

1. <span data-ttu-id="f3f9d-171">Sostituire il markup con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f3f9d-171">Replace the markup with the following code:</span></span>

[!code-razor[](signalr-blazor-webassembly/samples/3.x/BlazorSignalRApp/Client/Pages/Index.razor)]

## <a name="run-the-app"></a><span data-ttu-id="f3f9d-172">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="f3f9d-172">Run the app</span></span>

1. <span data-ttu-id="f3f9d-173">Seguire le istruzioni per gli strumenti:</span><span class="sxs-lookup"><span data-stu-id="f3f9d-173">Follow the guidance for your tooling:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="f3f9d-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f3f9d-174">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="f3f9d-175">In **Esplora soluzioni**selezionare il progetto **BlazorSignalRApp. Server** .</span><span class="sxs-lookup"><span data-stu-id="f3f9d-175">In **Solution Explorer**, select the **BlazorSignalRApp.Server** project.</span></span> <span data-ttu-id="f3f9d-176">Premere **CTRL + F5** per eseguire l'app senza debug.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-176">Press **Ctrl+F5** to run the app without debugging.</span></span>

1. <span data-ttu-id="f3f9d-177">Copiare l'URL dalla barra degli indirizzi, aprire un'altra istanza o scheda del browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-177">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="f3f9d-178">Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-178">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="f3f9d-179">Il nome e il messaggio vengono visualizzati immediatamente in entrambe le pagine:</span><span class="sxs-lookup"><span data-stu-id="f3f9d-179">The name and message are displayed on both pages instantly:</span></span>

   ![App di esempio webassembly di SignalR Blazer aperta in due finestre del browser che mostrano i messaggi scambiati.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="f3f9d-181">Virgolette: *Star Trek VI: il paese non individuato* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="f3f9d-181">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="f3f9d-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f3f9d-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="f3f9d-183">Selezionare **debug** > **Esegui senza eseguire debug** dalla barra degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-183">Select **Debug** > **Run Without Debugging** from the toolbar.</span></span>

1. <span data-ttu-id="f3f9d-184">Copiare l'URL dalla barra degli indirizzi, aprire un'altra istanza o scheda del browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-184">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="f3f9d-185">Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-185">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="f3f9d-186">Il nome e il messaggio vengono visualizzati immediatamente in entrambe le pagine:</span><span class="sxs-lookup"><span data-stu-id="f3f9d-186">The name and message are displayed on both pages instantly:</span></span>

   ![App di esempio webassembly di SignalR Blazer aperta in due finestre del browser che mostrano i messaggi scambiati.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="f3f9d-188">Virgolette: *Star Trek VI: il paese non individuato* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="f3f9d-188">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="f3f9d-189">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f3f9d-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="f3f9d-190">Nella barra laterale della **soluzione** selezionare il progetto **BlazorSignalRApp. Server** .</span><span class="sxs-lookup"><span data-stu-id="f3f9d-190">In the **Solution** sidebar, select the **BlazorSignalRApp.Server** project.</span></span> <span data-ttu-id="f3f9d-191">Dal menu selezionare **esegui** > **Avvia senza eseguire debug**.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-191">From the menu, select **Run** > **Start Without Debugging**.</span></span>

1. <span data-ttu-id="f3f9d-192">Copiare l'URL dalla barra degli indirizzi, aprire un'altra istanza o scheda del browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-192">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="f3f9d-193">Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-193">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="f3f9d-194">Il nome e il messaggio vengono visualizzati immediatamente in entrambe le pagine:</span><span class="sxs-lookup"><span data-stu-id="f3f9d-194">The name and message are displayed on both pages instantly:</span></span>

   ![App di esempio webassembly di SignalR Blazer aperta in due finestre del browser che mostrano i messaggi scambiati.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="f3f9d-196">Virgolette: *Star Trek VI: il paese non individuato* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="f3f9d-196">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="f3f9d-197">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="f3f9d-197">.NET Core CLI</span></span>](#tab/netcore-cli/)

1. <span data-ttu-id="f3f9d-198">In una shell dei comandi eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f3f9d-198">In a command shell, execute the following commands:</span></span>

   ```dotnetcli
   cd Server
   dotnet run
   ```

1. <span data-ttu-id="f3f9d-199">Copiare l'URL dalla barra degli indirizzi, aprire un'altra istanza o scheda del browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-199">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="f3f9d-200">Scegliere un browser, immettere un nome e un messaggio e fare clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="f3f9d-200">Choose either browser, enter a name and message, and select the **Send** button.</span></span> <span data-ttu-id="f3f9d-201">Il nome e il messaggio vengono visualizzati immediatamente in entrambe le pagine:</span><span class="sxs-lookup"><span data-stu-id="f3f9d-201">The name and message are displayed on both pages instantly:</span></span>

   ![App di esempio webassembly di SignalR Blazer aperta in due finestre del browser che mostrano i messaggi scambiati.](signalr-blazor-webassembly/_static/3.x/signalr-blazor-webassembly-finished.png)

   <span data-ttu-id="f3f9d-203">Virgolette: *Star Trek VI: il paese non individuato* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span><span class="sxs-lookup"><span data-stu-id="f3f9d-203">Quotes: *Star Trek VI: The Undiscovered Country* &copy;1991 [Paramount](https://www.paramountmovies.com/movies/star-trek-vi-the-undiscovered-country)</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="f3f9d-204">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="f3f9d-204">Next steps</span></span>

<span data-ttu-id="f3f9d-205">In questa esercitazione sono state illustrate le procedure per:</span><span class="sxs-lookup"><span data-stu-id="f3f9d-205">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f3f9d-206">Creare un progetto di app ospitata Blazor webassembly</span><span class="sxs-lookup"><span data-stu-id="f3f9d-206">Create a Blazor WebAssembly Hosted app project</span></span>
> * <span data-ttu-id="f3f9d-207">Aggiungere la libreria client di SignalR</span><span class="sxs-lookup"><span data-stu-id="f3f9d-207">Add the SignalR client library</span></span>
> * <span data-ttu-id="f3f9d-208">Aggiungere un hub SignalR</span><span class="sxs-lookup"><span data-stu-id="f3f9d-208">Add a SignalR hub</span></span>
> * <span data-ttu-id="f3f9d-209">Aggiungere SignalR Services e un endpoint per l'hub SignalR</span><span class="sxs-lookup"><span data-stu-id="f3f9d-209">Add SignalR services and an endpoint for the SignalR hub</span></span>
> * <span data-ttu-id="f3f9d-210">Aggiungere il codice del componente Razor per la chat</span><span class="sxs-lookup"><span data-stu-id="f3f9d-210">Add Razor component code for chat</span></span>

<span data-ttu-id="f3f9d-211">Per ulteriori informazioni sulla compilazione di app Blazor, vedere la documentazione di Blazor:</span><span class="sxs-lookup"><span data-stu-id="f3f9d-211">To learn more about building Blazor apps, see the Blazor documentation:</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/index>

## <a name="additional-resources"></a><span data-ttu-id="f3f9d-212">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f3f9d-212">Additional resources</span></span>

* <xref:signalr/introduction>
