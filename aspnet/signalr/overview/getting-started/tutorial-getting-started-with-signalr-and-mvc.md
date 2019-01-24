---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Esercitazione: Chat in tempo reale con SignalR 2 e MVC 5 | Microsoft Docs'
author: bradygaster
description: Questa esercitazione illustra come usare ASP.NET SignalR 2 per creare un'applicazione di chat in tempo reale. SignalR è aggiungere a un'applicazione MVC 5.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 1b02aecc68a93dbd6373ca5304530e76c9d0b6b5
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837001"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a><span data-ttu-id="fdc2f-104">Esercitazione: Chat in tempo reale con SignalR 2 e MVC 5</span><span class="sxs-lookup"><span data-stu-id="fdc2f-104">Tutorial: Real-time chat with SignalR 2 and MVC 5</span></span>

<span data-ttu-id="fdc2f-105">Questa esercitazione illustra come usare ASP.NET SignalR 2 per creare un'applicazione di chat in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-105">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="fdc2f-106">Aggiungere SignalR a un'applicazione MVC 5 e creare una vista di chat per inviare e visualizzare i messaggi.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-106">You add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span>

<span data-ttu-id="fdc2f-107">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="fdc2f-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fdc2f-108">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="fdc2f-108">Set up the project</span></span>
> * <span data-ttu-id="fdc2f-109">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="fdc2f-109">Run the sample</span></span>
> * <span data-ttu-id="fdc2f-110">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="fdc2f-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="fdc2f-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="fdc2f-111">Prerequisites</span></span>

* <span data-ttu-id="fdc2f-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con il **sviluppo ASP.NET e web** carico di lavoro.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="fdc2f-113">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="fdc2f-113">Set up the Project</span></span>

<span data-ttu-id="fdc2f-114">Questa sezione illustra come usare SignalR 2 e Visual Studio 2017 per creare un'applicazione ASP.NET MVC 5 vuota, aggiungere la libreria di SignalR e creare l'applicazione di chat.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="fdc2f-115">In Visual Studio, creare un'applicazione c# ASP.NET destinata a .NET Framework 4.5, denominarlo SignalRChat e fare clic su OK.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-115">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![Creazione di web](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. <span data-ttu-id="fdc2f-117">Nelle **nuova applicazione Web ASP.NET - SignalRMvcChat**, selezionare **MVC** e quindi selezionare **Modifica autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-117">In **New ASP.NET Web Application - SignalRMvcChat**, select **MVC** and then select **Change Authentication**.</span></span>

1. <span data-ttu-id="fdc2f-118">Nelle **Modifica autenticazione**, selezionare **Nessuna autenticazione** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-118">In **Change Authentication**, select **No Authentication** and click **OK**.</span></span>

    ![Seleziona Nessuna autenticazione](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. <span data-ttu-id="fdc2f-120">Nelle **nuova applicazione Web ASP.NET - SignalRMvcChat**, selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-120">In **New ASP.NET Web Application - SignalRMvcChat**, select **OK**.</span></span>

1. <span data-ttu-id="fdc2f-121">Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-121">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="fdc2f-122">Nelle **Aggiungi nuovo elemento - SignalRChat**, selezionare **installati** > **Visual C#**   >  **Web**  >  **SignalR** e quindi selezionare **classe tramite SignalR Hub (v2)**.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-122">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="fdc2f-123">Denominare la classe *ChatHub* e aggiungerlo al progetto.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-123">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="fdc2f-124">Questo passaggio Crea il *ChatHub.cs* file di classe e aggiunge un set di file di script e i riferimenti ad assembly che supportano SignalR al progetto.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-124">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="fdc2f-125">Sostituire il codice nella nuova *ChatHub.cs* file di classe con il seguente codice:</span><span class="sxs-lookup"><span data-stu-id="fdc2f-125">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. <span data-ttu-id="fdc2f-126">Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **classe**.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-126">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="fdc2f-127">Denominare la nuova classe *avvio* e aggiungerlo al progetto.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-127">Name the new class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="fdc2f-128">Sostituire il codice nel *Startup.cs* file di classe con il seguente codice:</span><span class="sxs-lookup"><span data-stu-id="fdc2f-128">Replace the code in the *Startup.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. <span data-ttu-id="fdc2f-129">Nelle **Esplora soluzioni**, selezionare **controller** > **HomeController.cs**.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-129">In **Solution Explorer**, select **Controllers** > **HomeController.cs**.</span></span>

1. <span data-ttu-id="fdc2f-130">Aggiungere questo metodo per la *HomeController.cs*.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-130">Add this method to the *HomeController.cs*.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    <span data-ttu-id="fdc2f-131">Questo metodo restituisce il **Chat** vista creata in un passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-131">This method returns the **Chat** view that you create in a later step.</span></span>

1. <span data-ttu-id="fdc2f-132">Nelle **Esplora soluzioni**, fare doppio clic su **viste** > **Home**e selezionare **Aggiungi**  >    **Visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-132">In **Solution Explorer**, right-click **Views** > **Home**, and select **Add** >  **View**.</span></span>

1. <span data-ttu-id="fdc2f-133">Nelle **Aggiungi visualizzazione**, assegnare un nome alla nuova visualizzazione **Chat** e selezionare **Add**.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-133">In **Add View**, name the new view **Chat** and select **Add**.</span></span>

1. <span data-ttu-id="fdc2f-134">Sostituire il contenuto del **Chat.cshtml** con questo codice:</span><span class="sxs-lookup"><span data-stu-id="fdc2f-134">Replace the contents of **Chat.cshtml** with this code:</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. <span data-ttu-id="fdc2f-135">Nelle **Esplora soluzioni**, espandere **script**.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-135">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="fdc2f-136">Librerie di script per jQuery e SignalR sono visibili nel progetto.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-136">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="fdc2f-137">La gestione dei pacchetti potrebbe avere installato una versione successiva degli script di SignalR.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-137">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="fdc2f-138">Verificare che i riferimenti a script nel blocco di codice corrispondono alle versioni dei file di script nel progetto.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-138">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="fdc2f-139">Riferimenti a script dal blocco di codice originale:</span><span class="sxs-lookup"><span data-stu-id="fdc2f-139">Script references from the original code block:</span></span>

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. <span data-ttu-id="fdc2f-140">Se non corrispondono, aggiornare il *cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-140">If they don't match, update the *.cshtml* file.</span></span>

1. <span data-ttu-id="fdc2f-141">Dalla barra dei menu, selezionare **File** > **Salva tutto**.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-141">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="fdc2f-142">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="fdc2f-142">Run the Sample</span></span>

1. <span data-ttu-id="fdc2f-143">Nella barra degli strumenti, attivare **debug degli Script** e quindi selezionare il pulsante play per eseguire l'esempio in modalità di Debug.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-143">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Immettere il nome utente](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. <span data-ttu-id="fdc2f-145">Quando si apre il browser, immettere un nome per l'identità di chat.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-145">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="fdc2f-146">Copiare l'URL dal browser, aprire due altri browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-146">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="fdc2f-147">In ogni browser e immettere un nome univoco.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-147">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="fdc2f-148">A questo punto, aggiungere un commento e selezionare **inviare**.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-148">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="fdc2f-149">Ripetere che in altri browser.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-149">Repeat that in the other browsers.</span></span> <span data-ttu-id="fdc2f-150">I commenti vengono visualizzati in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-150">The comments appear in real time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="fdc2f-151">Questa applicazione di chat semplice non gestisce il contesto di discussione sul server.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-151">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="fdc2f-152">L'hub trasmette i commenti per tutti gli utenti correnti.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-152">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="fdc2f-153">Gli utenti che partecipano alla chat in un secondo momento visualizzeranno messaggi aggiunti dal momento in cui che questi vengono aggiunti.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-153">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="fdc2f-154">Vedere come l'applicazione di chat viene eseguita in tre diversi browser.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-154">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="fdc2f-155">Per Susan, Anand e Tom invio di messaggi, aggiornare tutti i browser in tempo reale:</span><span class="sxs-lookup"><span data-stu-id="fdc2f-155">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Tutti i tre browser visualizzano alla stessa cronologia di chat](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. <span data-ttu-id="fdc2f-157">Nelle **Esplora soluzioni**, esaminare le **documenti Script** nodo per l'applicazione in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-157">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="fdc2f-158">È presente un file di script denominato *hub* che genera la libreria di SignalR in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-158">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="fdc2f-159">Questo file consente di gestire la comunicazione tra script jQuery e codice lato server.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-159">This file manages the communication between jQuery script and server-side code.</span></span>

    ![script hub generato automaticamente nel nodo documenti Script](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="fdc2f-161">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="fdc2f-161">Examine the Code</span></span>

<span data-ttu-id="fdc2f-162">L'applicazione di chat SignalR illustra due attività di sviluppo di SignalR base.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-162">The SignalR chat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="fdc2f-163">Viene illustrato come creare un hub.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-163">It shows you how to create a hub.</span></span> <span data-ttu-id="fdc2f-164">Il server utilizza tale hub dell'oggetto principale coordinamento.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-164">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="fdc2f-165">L'hub Usa la libreria jQuery SignalR per inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-165">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="fdc2f-166">Hub SignalR nel ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="fdc2f-166">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="fdc2f-167">Nell'esempio di codice, il `ChatHub` deriva dalla classe di `Microsoft.AspNet.SignalR.Hub` classe.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-167">In the code sample, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="fdc2f-168">Derivazione da di `Hub` classe è un modo utile per compilare un'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-168">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="fdc2f-169">È possibile creare metodi pubblici nella classe dell'hub e quindi accedere a tali metodi chiamandole dagli script in una pagina web.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-169">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="fdc2f-170">Nel codice di chat, i client chiamano il `ChatHub.Send` metodo per inviare un nuovo messaggio.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-170">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="fdc2f-171">L'hub, a sua volta invia il messaggio a tutti i client chiamando `Clients.All.addNewMessageToPage`.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-171">The hub in turn sends the message to all clients by calling `Clients.All.addNewMessageToPage`.</span></span>

<span data-ttu-id="fdc2f-172">Il `Send` metodo illustra alcuni concetti di hub:</span><span class="sxs-lookup"><span data-stu-id="fdc2f-172">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="fdc2f-173">Dichiarare i metodi pubblici in un hub in modo che i client possono chiamare li.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-173">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="fdc2f-174">Usare il `Microsoft.AspNet.SignalR.Hub.Clients` proprietà dinamica per comunicare con tutti i client connessi a questo hub.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-174">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="fdc2f-175">Chiamare una funzione nel client (ad esempio il `addNewMessageToPage` (funzione)) per aggiornare i client.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-175">Call a function on the client (like the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a><span data-ttu-id="fdc2f-176">SignalR e jQuery Chat.cshtml</span><span class="sxs-lookup"><span data-stu-id="fdc2f-176">SignalR and jQuery Chat.cshtml</span></span>

<span data-ttu-id="fdc2f-177">Il *Chat.cshtml* Visualizza file nell'esempio di codice illustra come usare la libreria jQuery SignalR per comunicare con un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-177">The *Chat.cshtml* view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span>  <span data-ttu-id="fdc2f-178">Il codice esegue molte attività importanti.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-178">The code carries out many important tasks.</span></span> <span data-ttu-id="fdc2f-179">Viene creato un riferimento al proxy generato automaticamente per l'hub e si dichiara una funzione che il server può chiamare per eseguire il push del contenuto ai client e avvia una connessione per inviare messaggi all'hub.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-179">It creates a reference to the autogenerated proxy for the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="fdc2f-180">In JavaScript, il riferimento alla classe server e i relativi membri è incluso camelCase.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-180">In JavaScript, the reference to the server class and its members is in camelCase.</span></span> <span data-ttu-id="fdc2f-181">I riferimenti di esempio di codice di C# `ChatHub` classi in JavaScript come `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-181">The code sample references the C# `ChatHub` class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="fdc2f-182">In questo blocco di codice, si crea una funzione di callback nello script.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-182">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="fdc2f-183">La classe dell'hub sul server chiama questa funzione per eseguire il push degli aggiornamenti di contenuto a ogni client.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-183">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="fdc2f-184">La chiamata facoltativa al `htmlEncode` funzione Mostra un modo per HTML codifica il contenuto del messaggio prima di visualizzarlo nella pagina.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-184">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page.</span></span> <span data-ttu-id="fdc2f-185">È un modo per impedire attacchi script injection.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-185">It's a way to prevent script injection.</span></span>

<span data-ttu-id="fdc2f-186">Questo codice consente di aprire una connessione con l'hub.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-186">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> <span data-ttu-id="fdc2f-187">Questo approccio assicura che si stabilisce una connessione prima dell'esecuzione il gestore dell'evento.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-187">This approach ensures that you establish a connection before the event handler executes.</span></span>

<span data-ttu-id="fdc2f-188">Il codice avvia la connessione e quindi lo si passa una funzione per gestire l'evento click sulle **inviare** pulsante nella pagina della Chat.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-188">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="fdc2f-189">Ottenere il codice</span><span class="sxs-lookup"><span data-stu-id="fdc2f-189">Get the code</span></span>

[<span data-ttu-id="fdc2f-190">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="fdc2f-190">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

## <a name="additional-resources"></a><span data-ttu-id="fdc2f-191">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="fdc2f-191">Additional resources</span></span>

<span data-ttu-id="fdc2f-192">Per altre informazioni su SignalR, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="fdc2f-192">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="fdc2f-193">Progetto di SignalR</span><span class="sxs-lookup"><span data-stu-id="fdc2f-193">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="fdc2f-194">SignalR GitHub ed esempi</span><span class="sxs-lookup"><span data-stu-id="fdc2f-194">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="fdc2f-195">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="fdc2f-195">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="fdc2f-196">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="fdc2f-196">Next steps</span></span>

<span data-ttu-id="fdc2f-197">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="fdc2f-197">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fdc2f-198">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="fdc2f-198">Set up the project</span></span>
> * <span data-ttu-id="fdc2f-199">È stato eseguito il codice di esempio</span><span class="sxs-lookup"><span data-stu-id="fdc2f-199">Ran the sample</span></span>
> * <span data-ttu-id="fdc2f-200">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="fdc2f-200">Examined the code</span></span>

<span data-ttu-id="fdc2f-201">Passare all'articolo successivo per informazioni su come creare un'applicazione web che usa ASP.NET SignalR 2 per fornire funzionalità di messaggistica ad alta frequenza.</span><span class="sxs-lookup"><span data-stu-id="fdc2f-201">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="fdc2f-202">App Web con la messaggistica ad alta frequenza</span><span class="sxs-lookup"><span data-stu-id="fdc2f-202">Web app with high-frequency messaging</span></span>](tutorial-high-frequency-realtime-with-signalr.md)