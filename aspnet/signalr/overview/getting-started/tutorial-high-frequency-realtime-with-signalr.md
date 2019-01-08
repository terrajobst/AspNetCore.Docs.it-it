---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Esercitazione: Creare app in tempo reale ad alta frequenza con SignalR 2 | Microsoft Docs'
author: pfletcher
description: Questa esercitazione illustra come creare un'applicazione web che usa ASP.NET SignalR per fornire funzionalità di messaggistica ad alta frequenza.
ms.author: riande
ms.date: 01/02/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 85503db0b41be6f87136627667d6dd71f0d4f609
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098590"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a><span data-ttu-id="76962-103">Esercitazione: Creare app in tempo reale ad alta frequenza con SignalR 2</span><span class="sxs-lookup"><span data-stu-id="76962-103">Tutorial: Create high-frequency real-time app with SignalR 2</span></span>

<span data-ttu-id="76962-104">Questa esercitazione illustra come creare un'applicazione web che usa ASP.NET SignalR 2 per fornire funzionalità di messaggistica ad alta frequenza.</span><span class="sxs-lookup"><span data-stu-id="76962-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span> <span data-ttu-id="76962-105">In questo caso, "messaggistica ad alta frequenza" significa che il server invia gli aggiornamenti a una tariffa fissa.</span><span class="sxs-lookup"><span data-stu-id="76962-105">In this case, "high-frequency messaging" means the server sends updates at a fixed rate.</span></span> <span data-ttu-id="76962-106">È inviare fino a 10 messaggi al secondo.</span><span class="sxs-lookup"><span data-stu-id="76962-106">You send up to 10 messages a second.</span></span>

<span data-ttu-id="76962-107">L'applicazione creata consente di visualizzare una forma che gli utenti potranno trascinare.</span><span class="sxs-lookup"><span data-stu-id="76962-107">The application you create displays a shape that users can drag.</span></span> <span data-ttu-id="76962-108">Il server Aggiorna la posizione della forma in tutti i browser connessi per corrispondere alla posizione della forma trascinata usando gli aggiornamenti programmati.</span><span class="sxs-lookup"><span data-stu-id="76962-108">The server updates the position of the shape in all connected browsers to match the position of the dragged shape using timed updates.</span></span>

<span data-ttu-id="76962-109">I concetti introdotti in questa esercitazione sono le applicazioni in modalità di gioco in tempo reale e altre applicazioni di simulazione.</span><span class="sxs-lookup"><span data-stu-id="76962-109">Concepts introduced in this tutorial have applications in real-time gaming and other simulation applications.</span></span>

<span data-ttu-id="76962-110">In questa esercitazione è:</span><span class="sxs-lookup"><span data-stu-id="76962-110">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="76962-111">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="76962-111">Set up the project</span></span>
> * <span data-ttu-id="76962-112">Creare l'applicazione di base</span><span class="sxs-lookup"><span data-stu-id="76962-112">Create the base application</span></span>
> * <span data-ttu-id="76962-113">Eseguire il mapping all'hub all'avvio di app</span><span class="sxs-lookup"><span data-stu-id="76962-113">Map to the hub when app starts</span></span>
> * <span data-ttu-id="76962-114">Aggiungere il client</span><span class="sxs-lookup"><span data-stu-id="76962-114">Add the client</span></span>
> * <span data-ttu-id="76962-115">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="76962-115">Run the app</span></span>
> * <span data-ttu-id="76962-116">Aggiungere il ciclo di client</span><span class="sxs-lookup"><span data-stu-id="76962-116">Add the client loop</span></span>
> * <span data-ttu-id="76962-117">Aggiungere il ciclo di server</span><span class="sxs-lookup"><span data-stu-id="76962-117">Add the server loop</span></span>
> * <span data-ttu-id="76962-118">Aggiungere animazioni uniformi</span><span class="sxs-lookup"><span data-stu-id="76962-118">Add smooth animation</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="76962-119">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="76962-119">Prerequisites</span></span>

* <span data-ttu-id="76962-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con il **sviluppo ASP.NET e web** carico di lavoro.</span><span class="sxs-lookup"><span data-stu-id="76962-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="76962-121">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="76962-121">Set up the project</span></span>

<span data-ttu-id="76962-122">In questa sezione si crea il progetto in Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="76962-122">In this section, you create the project in Visual Studio 2017.</span></span>

<span data-ttu-id="76962-123">Questa sezione illustra come usare Visual Studio 2017 per creare un'applicazione Web ASP.NET vuota e aggiungere le librerie di SignalR e jQuery.UI.</span><span class="sxs-lookup"><span data-stu-id="76962-123">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application and add the SignalR and jQuery.UI libraries.</span></span>

1. <span data-ttu-id="76962-124">In Visual Studio, creare un'applicazione Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="76962-124">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Creazione di web](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. <span data-ttu-id="76962-126">Nel **nuova applicazione Web ASP.NET - MoveShapeDemo** finestra, lasciare **vuota** selezionato e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="76962-126">In the **New ASP.NET Web Application - MoveShapeDemo** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="76962-127">Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="76962-127">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="76962-128">Nelle **Aggiungi nuovo elemento - MoveShapeDemo**, selezionare **installati** > **Visual C#**   >  **Web**  >  **SignalR** e quindi selezionare **classe tramite SignalR Hub (v2)**.</span><span class="sxs-lookup"><span data-stu-id="76962-128">In **Add New Item - MoveShapeDemo**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="76962-129">Denominare la classe *MoveShapeHub* e aggiungerlo al progetto.</span><span class="sxs-lookup"><span data-stu-id="76962-129">Name the class *MoveShapeHub* and add it to the project.</span></span>

    <span data-ttu-id="76962-130">Questo passaggio Crea il *MoveShapeHub.cs* file di classe.</span><span class="sxs-lookup"><span data-stu-id="76962-130">This step creates the *MoveShapeHub.cs* class file.</span></span> <span data-ttu-id="76962-131">Contemporaneamente, viene aggiunto un set di file di script e i riferimenti ad assembly che supportano SignalR al progetto.</span><span class="sxs-lookup"><span data-stu-id="76962-131">Simultaneously, it adds  a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="76962-132">Selezionare **strumenti di** > **Gestione pacchetti NuGet** > **la Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="76962-132">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

1. <span data-ttu-id="76962-133">Nelle **Console di gestione pacchetti**, eseguire questo comando:</span><span class="sxs-lookup"><span data-stu-id="76962-133">In **Package Manager Console**, run this command:</span></span>

    ```console
    Install-Package jQuery.UI.Combined
    ```

    <span data-ttu-id="76962-134">Il comando consente di installare la libreria dell'interfaccia utente di jQuery.</span><span class="sxs-lookup"><span data-stu-id="76962-134">The command installs the jQuery UI library.</span></span> <span data-ttu-id="76962-135">Utilizzarla per animare la forma.</span><span class="sxs-lookup"><span data-stu-id="76962-135">You use it to animate the shape.</span></span>

1. <span data-ttu-id="76962-136">Nelle **Esplora soluzioni**, espandere il nodo di script.</span><span class="sxs-lookup"><span data-stu-id="76962-136">In **Solution Explorer**, expand the Scripts node.</span></span>

    ![Riferimenti alla libreria di script](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    <span data-ttu-id="76962-138">Librerie di script per jQuery e jQueryUI SignalR sono visibili nel progetto.</span><span class="sxs-lookup"><span data-stu-id="76962-138">Script libraries for jQuery, jQueryUI, and SignalR are visible in the project.</span></span>

## <a name="create-the-base-application"></a><span data-ttu-id="76962-139">Creare l'applicazione di base</span><span class="sxs-lookup"><span data-stu-id="76962-139">Create the base application</span></span>

<span data-ttu-id="76962-140">In questa sezione si crea un'applicazione browser.</span><span class="sxs-lookup"><span data-stu-id="76962-140">In this section, you create a browser application.</span></span> <span data-ttu-id="76962-141">L'app invia la posizione della forma per il server durante ogni evento di spostamento del mouse.</span><span class="sxs-lookup"><span data-stu-id="76962-141">The app sends the location of the shape to the server during each mouse move event.</span></span> <span data-ttu-id="76962-142">Il server trasmette queste informazioni per tutti gli altri client connesso in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="76962-142">The server broadcasts this information to all other connected clients in real time.</span></span> <span data-ttu-id="76962-143">Altre informazioni sull'applicazione nelle sezioni successive.</span><span class="sxs-lookup"><span data-stu-id="76962-143">You learn more about this application in later sections.</span></span>

1. <span data-ttu-id="76962-144">Aprire il *MoveShapeHub.cs* file.</span><span class="sxs-lookup"><span data-stu-id="76962-144">Open the *MoveShapeHub.cs* file.</span></span>

1. <span data-ttu-id="76962-145">Sostituire il codice nel *MoveShapeHub.cs* file con il seguente codice:</span><span class="sxs-lookup"><span data-stu-id="76962-145">Replace the code in the *MoveShapeHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="76962-146">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="76962-146">Save the file.</span></span>

<span data-ttu-id="76962-147">Il `MoveShapeHub` classe è un'implementazione di un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="76962-147">The `MoveShapeHub` class is an implementation of a SignalR hub.</span></span> <span data-ttu-id="76962-148">Come nel [Introduzione a SignalR](tutorial-getting-started-with-signalr.md) esercitazione, l'hub ha un metodo che i client chiamano direttamente.</span><span class="sxs-lookup"><span data-stu-id="76962-148">As in the [Getting Started with SignalR](tutorial-getting-started-with-signalr.md) tutorial, the hub has a method that the clients call directly.</span></span> <span data-ttu-id="76962-149">In questo caso, il client invia un oggetto con il nuovo X e Y coordinate della forma per il server.</span><span class="sxs-lookup"><span data-stu-id="76962-149">In this case, the client sends an object with the new X and Y coordinates of the shape to the server.</span></span> <span data-ttu-id="76962-150">Queste coordinate ottengano sono trasmesse a tutti gli altri client connessi.</span><span class="sxs-lookup"><span data-stu-id="76962-150">Those coordinates get broadcasted to all other connected clients.</span></span> <span data-ttu-id="76962-151">SignalR serializza automaticamente questo oggetto tramite JSON.</span><span class="sxs-lookup"><span data-stu-id="76962-151">SignalR automatically serializes this object using JSON.</span></span>

<span data-ttu-id="76962-152">L'app invia la `ShapeModel` oggetto al client.</span><span class="sxs-lookup"><span data-stu-id="76962-152">The app sends the `ShapeModel` object to the client.</span></span> <span data-ttu-id="76962-153">Dispone di membri per archiviare la posizione della forma.</span><span class="sxs-lookup"><span data-stu-id="76962-153">It has members to store the position of the shape.</span></span> <span data-ttu-id="76962-154">La versione dell'oggetto sul server ha anche un membro per tenere traccia vengono archiviati dati del client.</span><span class="sxs-lookup"><span data-stu-id="76962-154">The version of the object on the server also has a member to track which client's data is being stored.</span></span> <span data-ttu-id="76962-155">Questo oggetto impedisce il server di inviare i dati del client a se stesso.</span><span class="sxs-lookup"><span data-stu-id="76962-155">This object prevents the server from sending a client's data back to itself.</span></span> <span data-ttu-id="76962-156">Questo membro viene utilizzato il `JsonIgnore` attributo per mantenere l'applicazione dalla serializzazione dei dati e lo invia al client.</span><span class="sxs-lookup"><span data-stu-id="76962-156">This member uses the `JsonIgnore` attribute to keep the application from serializing the data and sending it back to the client.</span></span>

## <a name="map-to-the-hub-when-app-starts"></a><span data-ttu-id="76962-157">Eseguire il mapping all'hub all'avvio di app</span><span class="sxs-lookup"><span data-stu-id="76962-157">Map to the hub when app starts</span></span>

<span data-ttu-id="76962-158">Successivamente, configurare il mapping all'hub all'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="76962-158">Next, you set up mapping to the hub when the application starts.</span></span> <span data-ttu-id="76962-159">In SignalR 2, l'aggiunta di una classe di avvio OWIN crea il mapping.</span><span class="sxs-lookup"><span data-stu-id="76962-159">In SignalR 2, adding an OWIN startup class creates the mapping.</span></span>

1. <span data-ttu-id="76962-160">Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="76962-160">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="76962-161">Nelle **Aggiungi nuovo elemento - MoveShapeDemo** selezionate **installati** > **Visual C#**   >  **Web** e quindi Selezionare **classe di avvio OWIN**.</span><span class="sxs-lookup"><span data-stu-id="76962-161">In **Add New Item - MoveShapeDemo** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="76962-162">Denominare la classe *avvio* e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="76962-162">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="76962-163">Sostituire il codice predefinito nel *Startup.cs* file con il seguente codice:</span><span class="sxs-lookup"><span data-stu-id="76962-163">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<span data-ttu-id="76962-164">Le chiamate di classe di avvio OWIN `MapSignalR` quando l'app viene eseguita la `Configuration` (metodo).</span><span class="sxs-lookup"><span data-stu-id="76962-164">The OWIN startup class calls `MapSignalR` when the app executes the `Configuration` method.</span></span> <span data-ttu-id="76962-165">L'app aggiunge la classe di avvio dell'OWIN elaborazione utilizzando la `OwinStartup` attributo dell'assembly.</span><span class="sxs-lookup"><span data-stu-id="76962-165">The app adds the class to OWIN's startup process using the `OwinStartup` assembly attribute.</span></span>

## <a name="add-the-client"></a><span data-ttu-id="76962-166">Aggiungere il client</span><span class="sxs-lookup"><span data-stu-id="76962-166">Add the client</span></span>

<span data-ttu-id="76962-167">Aggiungere la pagina HTML per il client.</span><span class="sxs-lookup"><span data-stu-id="76962-167">Add the HTML page for the client.</span></span>

1. <span data-ttu-id="76962-168">Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **pagina HTML**.</span><span class="sxs-lookup"><span data-stu-id="76962-168">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="76962-169">Denominare la pagina **predefinite** e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="76962-169">Name the page **Default** and select **OK**.</span></span>

1. <span data-ttu-id="76962-170">Nelle **Esplora soluzioni**, fare doppio clic su *default. HTML* e selezionare **imposta come pagina iniziale**.</span><span class="sxs-lookup"><span data-stu-id="76962-170">In **Solution Explorer**, right-click *Default.html* and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="76962-171">Sostituire il codice predefinito nel *default. HTML* file con il seguente codice:</span><span class="sxs-lookup"><span data-stu-id="76962-171">Replace the default code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. <span data-ttu-id="76962-172">Nelle **Esplora soluzioni**, espandere **script**.</span><span class="sxs-lookup"><span data-stu-id="76962-172">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="76962-173">Librerie di script per jQuery e SignalR sono visibili nel progetto.</span><span class="sxs-lookup"><span data-stu-id="76962-173">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="76962-174">La gestione dei pacchetti viene installata una versione successiva degli script di SignalR.</span><span class="sxs-lookup"><span data-stu-id="76962-174">The package manager installs a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="76962-175">Aggiornare i riferimenti di script nel blocco di codice in modo che corrispondano alle versioni dei file di script nel progetto.</span><span class="sxs-lookup"><span data-stu-id="76962-175">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

<span data-ttu-id="76962-176">Questo codice HTML e JavaScript consente di creare una linea rossa `div` chiamato `shape`.</span><span class="sxs-lookup"><span data-stu-id="76962-176">This HTML and JavaScript code creates a red `div` called `shape`.</span></span> <span data-ttu-id="76962-177">Abilita il comportamento di trascinamento della forma utilizzando la libreria jQuery e viene utilizzato il `drag` evento da inviare al server la posizione della forma.</span><span class="sxs-lookup"><span data-stu-id="76962-177">It enables the shape's dragging behavior using the jQuery library and uses the `drag` event to send the shape's position to the server.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="76962-178">Eseguire l'app</span><span class="sxs-lookup"><span data-stu-id="76962-178">Run the app</span></span>

<span data-ttu-id="76962-179">È possibile eseguire l'app a se'e funziona.</span><span class="sxs-lookup"><span data-stu-id="76962-179">You can run the app to se\`e it work.</span></span> <span data-ttu-id="76962-180">Quando si trascina la forma intorno a una finestra del browser, la forma viene spostato troppo in altri browser.</span><span class="sxs-lookup"><span data-stu-id="76962-180">When you drag the shape around a browser window, the shape moves in the other browsers too.</span></span>

1. <span data-ttu-id="76962-181">Nella barra degli strumenti, attivare **debug degli Script** e quindi selezionare il pulsante play per eseguire l'applicazione in modalità di Debug.</span><span class="sxs-lookup"><span data-stu-id="76962-181">In the toolbar, turn on **Script Debugging** and then select the play button to run the application in Debug mode.</span></span>

    ![Screenshot di utente attivando la modalità di debug e selezionare Riproduci.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    <span data-ttu-id="76962-183">Consente di aprire una finestra del browser con il cerchio rosso nell'angolo superiore destro.</span><span class="sxs-lookup"><span data-stu-id="76962-183">A browser window opens with the red shape in the upper-right corner.</span></span>

1. <span data-ttu-id="76962-184">Copiare l'URL della pagina.</span><span class="sxs-lookup"><span data-stu-id="76962-184">Copy the page's URL.</span></span>

1. <span data-ttu-id="76962-185">Aprire un altro browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="76962-185">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="76962-186">Trascinare la forma in una delle finestre del browser.</span><span class="sxs-lookup"><span data-stu-id="76962-186">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="76962-187">Segue la forma nella finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="76962-187">The shape in the other browser window follows.</span></span>

<span data-ttu-id="76962-188">Mentre l'applicazione funzioni con questo metodo, non è un modello di programmazione consigliato.</span><span class="sxs-lookup"><span data-stu-id="76962-188">While the application functions using this method, it's not a recommended programming model.</span></span> <span data-ttu-id="76962-189">Non vi è alcun limite al numero di messaggi inviati.</span><span class="sxs-lookup"><span data-stu-id="76962-189">There's no upper limit to the number of messages getting sent.</span></span> <span data-ttu-id="76962-190">Di conseguenza, i client e server ottenere sovraccaricati con messaggi e comporta una riduzione delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="76962-190">As a result, the clients and server get overwhelmed with messages and performance degrades.</span></span> <span data-ttu-id="76962-191">Inoltre, l'app Visualizza un'animazione indipendente sul client.</span><span class="sxs-lookup"><span data-stu-id="76962-191">Also, the app displays a disjointed animation on the client.</span></span> <span data-ttu-id="76962-192">Questa animazione scatti accade perché la forma si sposta immediatamente da ogni metodo.</span><span class="sxs-lookup"><span data-stu-id="76962-192">This jerky animation happens because the shape moves instantly by each method.</span></span> <span data-ttu-id="76962-193">È preferibile se la forma viene spostata in modo uniforme in ogni nuova posizione.</span><span class="sxs-lookup"><span data-stu-id="76962-193">It's better if the shape moves smoothly to each new location.</span></span> <span data-ttu-id="76962-194">Successivamente, descrive come risolvere questi problemi.</span><span class="sxs-lookup"><span data-stu-id="76962-194">Next, you learn how to fix those issues.</span></span>

## <a name="add-the-client-loop"></a><span data-ttu-id="76962-195">Aggiungere il ciclo di client</span><span class="sxs-lookup"><span data-stu-id="76962-195">Add the client loop</span></span>

<span data-ttu-id="76962-196">Inviare la posizione della forma per ogni evento di spostamento del mouse, viene creata una quantità di traffico di rete non necessaria.</span><span class="sxs-lookup"><span data-stu-id="76962-196">Sending the location of the shape on every mouse move event creates an unnecessary amount of network traffic.</span></span> <span data-ttu-id="76962-197">L'app deve limitare i messaggi dal client.</span><span class="sxs-lookup"><span data-stu-id="76962-197">The app needs to throttle the messages from the client.</span></span>

<span data-ttu-id="76962-198">Usare il codice javascript `setInterval` funzione per impostare un ciclo che invia informazioni sulla posizione di nuovo al server a una tariffa fissa.</span><span class="sxs-lookup"><span data-stu-id="76962-198">Use the javascript `setInterval` function to set up a loop that sends new position information to the server at a fixed rate.</span></span> <span data-ttu-id="76962-199">Il ciclo è una rappresentazione di base di un "ciclo del gioco".</span><span class="sxs-lookup"><span data-stu-id="76962-199">This loop is a basic representation of a "game loop."</span></span> <span data-ttu-id="76962-200">È una funzione chiamata più volte che controlla tutte le funzionalità di un gioco.</span><span class="sxs-lookup"><span data-stu-id="76962-200">It's a repeatedly called function that drives all the functionality of a game.</span></span>

1. <span data-ttu-id="76962-201">Sostituire il codice client di *default. HTML* file con il seguente codice:</span><span class="sxs-lookup"><span data-stu-id="76962-201">Replace the client code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > <span data-ttu-id="76962-202">È necessario sostituire i riferimenti a script nuovamente.</span><span class="sxs-lookup"><span data-stu-id="76962-202">You have to replace the script references again.</span></span> <span data-ttu-id="76962-203">Le versioni degli script nel progetto devono corrispondere.</span><span class="sxs-lookup"><span data-stu-id="76962-203">They must match the versions of the scripts in the project.</span></span>

    <span data-ttu-id="76962-204">Questo nuovo codice aggiunge il `updateServerModel` (funzione).</span><span class="sxs-lookup"><span data-stu-id="76962-204">This new code adds the `updateServerModel` function.</span></span> <span data-ttu-id="76962-205">Essere stato chiamato in base a una frequenza fissa.</span><span class="sxs-lookup"><span data-stu-id="76962-205">It gets called on a fixed frequency.</span></span> <span data-ttu-id="76962-206">La funzione Invia i dati di posizione per il server ogni volta che il `moved` flag indica che sono disponibili nuovi dati di posizione per inviare.</span><span class="sxs-lookup"><span data-stu-id="76962-206">The function sends the position data to the server whenever the `moved` flag indicates that there's new position data to send.</span></span>

1. <span data-ttu-id="76962-207">Selezionare il pulsante di riproduzione per avviare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="76962-207">Select the play button to start the application</span></span>

1. <span data-ttu-id="76962-208">Copiare l'URL della pagina.</span><span class="sxs-lookup"><span data-stu-id="76962-208">Copy the page's URL.</span></span>

1. <span data-ttu-id="76962-209">Aprire un altro browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="76962-209">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="76962-210">Trascinare la forma in una delle finestre del browser.</span><span class="sxs-lookup"><span data-stu-id="76962-210">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="76962-211">Segue la forma nella finestra del browser.</span><span class="sxs-lookup"><span data-stu-id="76962-211">The shape in the other browser window follows.</span></span>

<span data-ttu-id="76962-212">Poiché l'app limita il numero di messaggi che vengono inviate al server, l'animazione non sarà più visualizzato come smooth ha inizialmente.</span><span class="sxs-lookup"><span data-stu-id="76962-212">Since the app throttles the number of messages that get sent to the server, the animation won't appear as smooth did at first.</span></span>

## <a name="add-the-server-loop"></a><span data-ttu-id="76962-213">Aggiungere il ciclo di server</span><span class="sxs-lookup"><span data-stu-id="76962-213">Add the server loop</span></span>

<span data-ttu-id="76962-214">Nell'applicazione corrente, i messaggi inviati dal server al client porto spesso loro ricezione.</span><span class="sxs-lookup"><span data-stu-id="76962-214">In the current application, messages sent from the server to the client go out as often as they're received.</span></span> <span data-ttu-id="76962-215">Il traffico di rete presenta un problema analogo come illustrato nel client.</span><span class="sxs-lookup"><span data-stu-id="76962-215">This network traffic presents a similar problem as we see on the client.</span></span>

<span data-ttu-id="76962-216">L'app può inviare messaggi più spesso sono necessari.</span><span class="sxs-lookup"><span data-stu-id="76962-216">The app can send messages more often than they're needed.</span></span> <span data-ttu-id="76962-217">Di conseguenza, la connessione può diventare diffusi.</span><span class="sxs-lookup"><span data-stu-id="76962-217">The connection can become flooded as a result.</span></span> <span data-ttu-id="76962-218">In questa sezione viene descritto come aggiornare il server per aggiungere un timer che limita la frequenza dei messaggi in uscita.</span><span class="sxs-lookup"><span data-stu-id="76962-218">This section describes how to update the server to add a timer that throttles the rate of the outgoing messages.</span></span>

1. <span data-ttu-id="76962-219">Sostituire il contenuto di `MoveShapeHub.cs` con questo codice:</span><span class="sxs-lookup"><span data-stu-id="76962-219">Replace the contents of `MoveShapeHub.cs` with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. <span data-ttu-id="76962-220">Selezionare il pulsante di riproduzione per avviare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="76962-220">Select the play button to start the application.</span></span>

1. <span data-ttu-id="76962-221">Copiare l'URL della pagina.</span><span class="sxs-lookup"><span data-stu-id="76962-221">Copy the page's URL.</span></span>

1. <span data-ttu-id="76962-222">Aprire un altro browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="76962-222">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="76962-223">Trascinare la forma in una delle finestre del browser.</span><span class="sxs-lookup"><span data-stu-id="76962-223">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="76962-224">Questo codice si espande il client per aggiungere il `Broadcaster` classe.</span><span class="sxs-lookup"><span data-stu-id="76962-224">This code expands the client to add the `Broadcaster` class.</span></span> <span data-ttu-id="76962-225">La nuova classe limita i messaggi in uscita mediante il `Timer` classe da .NET framework.</span><span class="sxs-lookup"><span data-stu-id="76962-225">The new class throttles the outgoing messages using the `Timer` class from the .NET framework.</span></span>

<span data-ttu-id="76962-226">È consigliabile sapere che lo stesso hub è transitorio.</span><span class="sxs-lookup"><span data-stu-id="76962-226">It's good to learn that the hub itself is transitory.</span></span> <span data-ttu-id="76962-227">Viene creato ogni volta che è necessaria.</span><span class="sxs-lookup"><span data-stu-id="76962-227">It's created every time it's needed.</span></span> <span data-ttu-id="76962-228">In modo che l'app crea le `Broadcaster` come singleton.</span><span class="sxs-lookup"><span data-stu-id="76962-228">So the app creates the `Broadcaster` as a singleton.</span></span> <span data-ttu-id="76962-229">Usa l'inizializzazione differita per rinviare la `Broadcaster`della creazione fino a quando non è necessaria.</span><span class="sxs-lookup"><span data-stu-id="76962-229">It uses lazy initialization to defer the `Broadcaster`'s creation until it's needed.</span></span> <span data-ttu-id="76962-230">In questo modo si garantisce che l'app viene creata la prima istanza di hub completamente prima di avviare il timer.</span><span class="sxs-lookup"><span data-stu-id="76962-230">That guarantees that the app creates the first hub instance completely before starting the timer.</span></span>

<span data-ttu-id="76962-231">La chiamata a dei client `UpdateShape` funzione viene quindi spostata all'esterno dell'hub `UpdateModel` (metodo).</span><span class="sxs-lookup"><span data-stu-id="76962-231">The call to the clients' `UpdateShape` function is then moved out of the hub's `UpdateModel` method.</span></span> <span data-ttu-id="76962-232">Non è più viene chiamato immediatamente ogni volta che l'app riceve i messaggi in arrivo.</span><span class="sxs-lookup"><span data-stu-id="76962-232">It's no longer called immediately whenever the app receives incoming messages.</span></span> <span data-ttu-id="76962-233">Al contrario, l'app invia i messaggi per i client con una frequenza pari a 25 chiamate al secondo.</span><span class="sxs-lookup"><span data-stu-id="76962-233">Instead, the app sends the messages to the clients at a rate of 25 calls per second.</span></span> <span data-ttu-id="76962-234">Il processo viene gestito dal `_broadcastLoop` timer dall'interno di `Broadcaster` classe.</span><span class="sxs-lookup"><span data-stu-id="76962-234">The process is  managed by the `_broadcastLoop` timer from within the `Broadcaster` class.</span></span>

<span data-ttu-id="76962-235">Infine, invece di chiamare il metodo client dall'hub direttamente, il `Broadcaster` classe deve ottenere un riferimento al sistema attualmente `_hubContext` hub.</span><span class="sxs-lookup"><span data-stu-id="76962-235">Lastly, instead of calling the client method from the hub directly, the `Broadcaster` class needs to get a reference to the currently operating `_hubContext` hub.</span></span> <span data-ttu-id="76962-236">Ottiene il riferimento con il `GlobalHost`.</span><span class="sxs-lookup"><span data-stu-id="76962-236">It gets the reference with the `GlobalHost`.</span></span>

## <a name="add-smooth-animation"></a><span data-ttu-id="76962-237">Aggiungere animazioni uniformi</span><span class="sxs-lookup"><span data-stu-id="76962-237">Add smooth animation</span></span>

<span data-ttu-id="76962-238">L'applicazione è quasi terminata, ma possiamo rendere un miglioramento più.</span><span class="sxs-lookup"><span data-stu-id="76962-238">The application is almost finished, but we could make one more improvement.</span></span> <span data-ttu-id="76962-239">L'app consente di spostare la forma sul client in risposta ai messaggi di server.</span><span class="sxs-lookup"><span data-stu-id="76962-239">The app moves the shape on the client in response to server messages.</span></span> <span data-ttu-id="76962-240">Anziché impostare la posizione della forma per il nuovo percorso assegnato dal server, usare la libreria di JQuery UI `animate` (funzione).</span><span class="sxs-lookup"><span data-stu-id="76962-240">Instead of setting the position of the shape to the new location given by the server, use the JQuery UI library's `animate` function.</span></span> <span data-ttu-id="76962-241">È possibile spostare la forma in modo uniforme tra la posizione corrente e nuovo.</span><span class="sxs-lookup"><span data-stu-id="76962-241">It can move the shape smoothly between its current and new position.</span></span>

1. <span data-ttu-id="76962-242">Aggiornare il client `updateShape` metodo nella *default. HTML* file simile al codice evidenziato di seguito:</span><span class="sxs-lookup"><span data-stu-id="76962-242">Update the client's `updateShape` method in the *Default.html* file to look like the highlighted code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. <span data-ttu-id="76962-243">Selezionare il pulsante di riproduzione per avviare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="76962-243">Select the play button to start the application.</span></span>

1. <span data-ttu-id="76962-244">Copiare l'URL della pagina.</span><span class="sxs-lookup"><span data-stu-id="76962-244">Copy the page's URL.</span></span>

1. <span data-ttu-id="76962-245">Aprire un altro browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="76962-245">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="76962-246">Trascinare la forma in una delle finestre del browser.</span><span class="sxs-lookup"><span data-stu-id="76962-246">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="76962-247">Lo spostamento della forma in altra finestra meno fluido.</span><span class="sxs-lookup"><span data-stu-id="76962-247">The movement of the shape in the other window appears less jerky.</span></span> <span data-ttu-id="76962-248">L'app crea un'interpolazione suo movimento nel tempo, piuttosto che viene impostata una sola volta per ogni messaggio in arrivo.</span><span class="sxs-lookup"><span data-stu-id="76962-248">The app interpolates its movement over time rather than being set once per incoming message.</span></span>

<span data-ttu-id="76962-249">Questo codice consente di spostare la forma dalla posizione precedente a quello nuovo.</span><span class="sxs-lookup"><span data-stu-id="76962-249">This code moves the shape from the old location to the new one.</span></span> <span data-ttu-id="76962-250">Il server indica la posizione della forma nel corso dell'intervallo di animazione.</span><span class="sxs-lookup"><span data-stu-id="76962-250">The server gives the position of the shape over the course of the animation interval.</span></span> <span data-ttu-id="76962-251">In questo caso, che è 100 millisecondi.</span><span class="sxs-lookup"><span data-stu-id="76962-251">In this case, that's 100 milliseconds.</span></span> <span data-ttu-id="76962-252">L'app consente di cancellare qualsiasi animazione precedente in esecuzione in una forma prima che inizi la nuova animazione.</span><span class="sxs-lookup"><span data-stu-id="76962-252">The app clears any previous animation running on the shape before the new animation starts.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="76962-253">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="76962-253">Additional resources</span></span>

<span data-ttu-id="76962-254">È utile per lo sviluppo di giochi online e altre simulazioni, ad esempio si è appena appreso come il paradigma comune della comunicazione [gioco ShootR creato con SignalR](https://shootr.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="76962-254">The communication paradigm you just learned about is useful for developing online games and other simulations, like [the ShootR game created with SignalR](https://shootr.azurewebsites.net/).</span></span>

<span data-ttu-id="76962-255">Per altre informazioni su SignalR, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="76962-255">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="76962-256">Progetto di SignalR</span><span class="sxs-lookup"><span data-stu-id="76962-256">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="76962-257">SignalR GitHub ed esempi</span><span class="sxs-lookup"><span data-stu-id="76962-257">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="76962-258">Wiki di SignalR</span><span class="sxs-lookup"><span data-stu-id="76962-258">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="76962-259">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="76962-259">Next steps</span></span>

<span data-ttu-id="76962-260">In questa esercitazione è:</span><span class="sxs-lookup"><span data-stu-id="76962-260">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="76962-261">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="76962-261">Set up the project</span></span>
> * <span data-ttu-id="76962-262">Creazione dell'applicazione di base</span><span class="sxs-lookup"><span data-stu-id="76962-262">Created the base application</span></span>
> * <span data-ttu-id="76962-263">Il mapping all'hub all'avvio di app</span><span class="sxs-lookup"><span data-stu-id="76962-263">Mapped to the hub when app starts</span></span>
> * <span data-ttu-id="76962-264">Aggiunta del client</span><span class="sxs-lookup"><span data-stu-id="76962-264">Added the client</span></span>
> * <span data-ttu-id="76962-265">È stata eseguita l'app</span><span class="sxs-lookup"><span data-stu-id="76962-265">Ran the app</span></span>
> * <span data-ttu-id="76962-266">Aggiunto il ciclo di client</span><span class="sxs-lookup"><span data-stu-id="76962-266">Added the client loop</span></span>
> * <span data-ttu-id="76962-267">Aggiunta di ciclo del server</span><span class="sxs-lookup"><span data-stu-id="76962-267">Added the server loop</span></span>
> * <span data-ttu-id="76962-268">Aggiunta di animazioni uniformi</span><span class="sxs-lookup"><span data-stu-id="76962-268">Added smooth animation</span></span>

<span data-ttu-id="76962-269">Passare all'articolo successivo per informazioni su come creare un'applicazione web che usa ASP.NET SignalR 2 per fornire funzionalità di trasmissione di server.</span><span class="sxs-lookup"><span data-stu-id="76962-269">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="76962-270">SignalR 2 e la trasmissione di server</span><span class="sxs-lookup"><span data-stu-id="76962-270">SignalR 2 and server broadcasting</span></span>](tutorial-server-broadcast-with-signalr.md)