---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Esercitazione: Chat in tempo reale con SignalR 2 | Microsoft Docs'
author: pfletcher
description: In questa esercitazione viene illustrato come usare SignalR per creare un'applicazione di chat in tempo reale. SignalR è aggiungere a un'applicazione web ASP.NET vuota.
ms.author: riande
ms.date: 01/02/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: aa015abc47bb2450e04e167c0404aaa1d119ba2c
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098624"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a><span data-ttu-id="112be-104">Esercitazione: Chat in tempo reale con SignalR 2</span><span class="sxs-lookup"><span data-stu-id="112be-104">Tutorial: Real-time chat with SignalR 2</span></span>

<span data-ttu-id="112be-105">Questa esercitazione illustra come usare SignalR per creare un'applicazione di chat in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="112be-105">This tutorial shows you how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="112be-106">Aggiungere SignalR a un'applicazione web ASP.NET vuota e creare una pagina HTML per l'invio e visualizzare i messaggi.</span><span class="sxs-lookup"><span data-stu-id="112be-106">You add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>

<span data-ttu-id="112be-107">In questa esercitazione è:</span><span class="sxs-lookup"><span data-stu-id="112be-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="112be-108">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="112be-108">Set up the project</span></span>
> * <span data-ttu-id="112be-109">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="112be-109">Run the sample</span></span>
> * <span data-ttu-id="112be-110">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="112be-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="112be-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="112be-111">Prerequisites</span></span>

* <span data-ttu-id="112be-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con il **sviluppo ASP.NET e web** carico di lavoro.</span><span class="sxs-lookup"><span data-stu-id="112be-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="112be-113">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="112be-113">Set up the Project</span></span>

<span data-ttu-id="112be-114">Questa sezione illustra come usare SignalR 2 e Visual Studio 2017 per creare un'applicazione web ASP.NET vuota, aggiungere SignalR e creare l'applicazione di chat.</span><span class="sxs-lookup"><span data-stu-id="112be-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

1. <span data-ttu-id="112be-115">In Visual Studio, creare un'applicazione Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="112be-115">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Creazione di web](tutorial-getting-started-with-signalr/_static/image2.png)

1. <span data-ttu-id="112be-117">Nel **nuovo progetto ASP.NET - SignalRChat** finestra, lasciare **vuota** selezionato e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="112be-117">In the **New ASP.NET Project - SignalRChat** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="112be-118">Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="112be-118">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="112be-119">Nelle **Aggiungi nuovo elemento - SignalRChat**, selezionare **installati** > **Visual C#**   >  **Web**  >  **SignalR** e quindi selezionare **classe tramite SignalR Hub (v2)**.</span><span class="sxs-lookup"><span data-stu-id="112be-119">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="112be-120">Denominare la classe *ChatHub* e aggiungerlo al progetto.</span><span class="sxs-lookup"><span data-stu-id="112be-120">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="112be-121">Questo passaggio Crea il *ChatHub.cs* file di classe e aggiunge un set di file di script e i riferimenti ad assembly che supportano SignalR al progetto.</span><span class="sxs-lookup"><span data-stu-id="112be-121">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="112be-122">Sostituire il codice nella nuova *ChatHub.cs* file di classe con il seguente codice:</span><span class="sxs-lookup"><span data-stu-id="112be-122">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="112be-123">Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="112be-123">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="112be-124">Nelle **Aggiungi nuovo elemento - SignalRChat** selezionate **installati** > **Visual C#**   >  **Web** e quindi Selezionare **classe di avvio OWIN**.</span><span class="sxs-lookup"><span data-stu-id="112be-124">In **Add New Item - SignalRChat** select **Installed** > **Visual C#** > **Web**  and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="112be-125">Denominare la classe *avvio* e aggiungerlo al progetto.</span><span class="sxs-lookup"><span data-stu-id="112be-125">Name the class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="112be-126">Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Add** > **pagina HTML**.</span><span class="sxs-lookup"><span data-stu-id="112be-126">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="112be-127">Denominare la nuova pagina *indice* e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="112be-127">Name the new page *index* and select **OK**.</span></span>

1. <span data-ttu-id="112be-128">Nelle **Esplora soluzioni**, la pagina HTML creata e scegliere **imposta come pagina iniziale**.</span><span class="sxs-lookup"><span data-stu-id="112be-128">In **Solution Explorer**, right-click the HTML page you created and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="112be-129">Sostituire il codice predefinito nella pagina HTML con il seguente codice:</span><span class="sxs-lookup"><span data-stu-id="112be-129">Replace the default code in the HTML page with this code:</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. <span data-ttu-id="112be-130">Nelle **Esplora soluzioni**, espandere **script**.</span><span class="sxs-lookup"><span data-stu-id="112be-130">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="112be-131">Librerie di script per jQuery e SignalR sono visibili nel progetto.</span><span class="sxs-lookup"><span data-stu-id="112be-131">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="112be-132">La gestione dei pacchetti potrebbe avere installato una versione successiva degli script di SignalR.</span><span class="sxs-lookup"><span data-stu-id="112be-132">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="112be-133">Verificare che i riferimenti a script nel blocco di codice corrispondono alle versioni dei file di script nel progetto.</span><span class="sxs-lookup"><span data-stu-id="112be-133">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="112be-134">Riferimenti a script dal blocco di codice originale:</span><span class="sxs-lookup"><span data-stu-id="112be-134">Script references from the original code block:</span></span>
    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. <span data-ttu-id="112be-135">Se non corrispondono, aggiornare il *HTML* file.</span><span class="sxs-lookup"><span data-stu-id="112be-135">If they don't match, update the *.html* file.</span></span>

1. <span data-ttu-id="112be-136">Dalla barra dei menu, selezionare **File** > **Salva tutto**.</span><span class="sxs-lookup"><span data-stu-id="112be-136">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="112be-137">Eseguire l'esempio</span><span class="sxs-lookup"><span data-stu-id="112be-137">Run the Sample</span></span>

1. <span data-ttu-id="112be-138">Nella barra degli strumenti, attivare **debug degli Script** e quindi selezionare il pulsante play per eseguire l'esempio in modalità di Debug.</span><span class="sxs-lookup"><span data-stu-id="112be-138">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Immettere il nome utente](tutorial-getting-started-with-signalr/_static/image3.png)

1. <span data-ttu-id="112be-140">Quando si apre il browser, immettere un nome per l'identità di chat.</span><span class="sxs-lookup"><span data-stu-id="112be-140">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="112be-141">Copiare l'URL dal browser, aprire due altri browser e incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="112be-141">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="112be-142">In ogni browser e immettere un nome univoco.</span><span class="sxs-lookup"><span data-stu-id="112be-142">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="112be-143">A questo punto, aggiungere un commento e selezionare **inviare**.</span><span class="sxs-lookup"><span data-stu-id="112be-143">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="112be-144">Ripetere che in altri browser.</span><span class="sxs-lookup"><span data-stu-id="112be-144">Repeat that in the other browsers.</span></span> <span data-ttu-id="112be-145">I commenti vengono visualizzati in tempo reale.</span><span class="sxs-lookup"><span data-stu-id="112be-145">The comments appear in real-time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="112be-146">Questa applicazione di chat semplice non gestisce il contesto di discussione sul server.</span><span class="sxs-lookup"><span data-stu-id="112be-146">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="112be-147">L'hub trasmette i commenti per tutti gli utenti correnti.</span><span class="sxs-lookup"><span data-stu-id="112be-147">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="112be-148">Gli utenti che partecipano alla chat in un secondo momento visualizzeranno messaggi aggiunti dal momento in cui che questi vengono aggiunti.</span><span class="sxs-lookup"><span data-stu-id="112be-148">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="112be-149">Vedere come l'applicazione di chat viene eseguita in tre diversi browser.</span><span class="sxs-lookup"><span data-stu-id="112be-149">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="112be-150">Per Susan, Anand e Tom invio di messaggi, aggiornare tutti i browser in tempo reale:</span><span class="sxs-lookup"><span data-stu-id="112be-150">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Tutti i tre browser visualizzano alla stessa cronologia di chat](tutorial-getting-started-with-signalr/_static/image4.png)

1. <span data-ttu-id="112be-152">Nelle **Esplora soluzioni**, esaminare le **documenti Script** nodo per l'applicazione in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="112be-152">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="112be-153">È presente un file di script denominato *hub* che genera la libreria di SignalR in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="112be-153">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="112be-154">Questo file consente di gestire la comunicazione tra script jQuery e codice lato server.</span><span class="sxs-lookup"><span data-stu-id="112be-154">This file manages the communication between jQuery script and server-side code.</span></span>

    ![script hub generato automaticamente nel nodo documenti Script](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="112be-156">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="112be-156">Examine the Code</span></span>

<span data-ttu-id="112be-157">L'applicazione SignalRChat illustra due attività di sviluppo di SignalR base.</span><span class="sxs-lookup"><span data-stu-id="112be-157">The SignalRChat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="112be-158">Viene illustrato come creare un hub.</span><span class="sxs-lookup"><span data-stu-id="112be-158">It shows you how to create a hub.</span></span> <span data-ttu-id="112be-159">Il server utilizza tale hub dell'oggetto principale coordinamento.</span><span class="sxs-lookup"><span data-stu-id="112be-159">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="112be-160">L'hub Usa la libreria jQuery SignalR per inviare e ricevere messaggi.</span><span class="sxs-lookup"><span data-stu-id="112be-160">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="112be-161">Hub SignalR nel ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="112be-161">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="112be-162">Nell'esempio di codice precedente, il `ChatHub` deriva dalla classe di `Microsoft.AspNet.SignalR.Hub` classe.</span><span class="sxs-lookup"><span data-stu-id="112be-162">In the code sample above, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="112be-163">Derivazione da di `Hub` classe è un modo utile per compilare un'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="112be-163">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="112be-164">È possibile creare metodi pubblici nella classe dell'hub e quindi usare tali metodi chiamandole dagli script in una pagina web.</span><span class="sxs-lookup"><span data-stu-id="112be-164">You can create public methods on your hub class and then use those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="112be-165">Nel codice di chat, i client chiamano il `ChatHub.Send` metodo per inviare un nuovo messaggio.</span><span class="sxs-lookup"><span data-stu-id="112be-165">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="112be-166">L'hub invia quindi il messaggio a tutti i client chiamando `Clients.All.broadcastMessage`.</span><span class="sxs-lookup"><span data-stu-id="112be-166">The hub then sends the message to all clients by calling `Clients.All.broadcastMessage`.</span></span>

<span data-ttu-id="112be-167">Il `Send` metodo illustra alcuni concetti di hub:</span><span class="sxs-lookup"><span data-stu-id="112be-167">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="112be-168">Dichiarare i metodi pubblici in un hub in modo che i client possono chiamare li.</span><span class="sxs-lookup"><span data-stu-id="112be-168">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="112be-169">Usare il `Microsoft.AspNet.SignalR.Hub.Clients` proprietà dinamica per comunicare con tutti i client connessi a questo hub.</span><span class="sxs-lookup"><span data-stu-id="112be-169">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="112be-170">Chiamare una funzione nel client (ad esempio il `broadcastMessage` (funzione)) per aggiornare i client.</span><span class="sxs-lookup"><span data-stu-id="112be-170">Call a function on the client (like the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a><span data-ttu-id="112be-171">SignalR e jQuery in Index. HTML</span><span class="sxs-lookup"><span data-stu-id="112be-171">SignalR and jQuery in the index.html</span></span>

<span data-ttu-id="112be-172">Il *index. HTML* pagina nell'esempio di codice illustra come usare la libreria jQuery SignalR per comunicare con un hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="112be-172">The *index.html* page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="112be-173">Il codice esegue molte attività importanti.</span><span class="sxs-lookup"><span data-stu-id="112be-173">The code carries out many important tasks.</span></span> <span data-ttu-id="112be-174">Dichiara un proxy per l'hub di riferimento, si dichiara una funzione che il server può chiamare per contenuto push ai client e avvia una connessione per inviare messaggi all'hub.</span><span class="sxs-lookup"><span data-stu-id="112be-174">It declares a proxy to reference the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="112be-175">In JavaScript deve essere camelCase il riferimento alla classe server e i relativi membri.</span><span class="sxs-lookup"><span data-stu-id="112be-175">In JavaScript the reference to the server class and its members has to be camelCase.</span></span> <span data-ttu-id="112be-176">I riferimenti di esempio di codice di C# *ChatHub* classi in JavaScript come `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="112be-176">The code sample references the C# *ChatHub* class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="112be-177">In questo blocco di codice, si crea una funzione di callback nello script.</span><span class="sxs-lookup"><span data-stu-id="112be-177">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="112be-178">La classe dell'hub sul server chiama questa funzione per eseguire il push degli aggiornamenti di contenuto a ogni client.</span><span class="sxs-lookup"><span data-stu-id="112be-178">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="112be-179">Le due righe di tale codifica HTML il contenuto prima di visualizzarla sono facoltativi e Mostra un buon metodo per impedire l'inserimento di script.</span><span class="sxs-lookup"><span data-stu-id="112be-179">The two lines that HTML-encode the content before displaying it are optional and show a good way to prevent script injection.</span></span>

<span data-ttu-id="112be-180">Questo codice consente di aprire una connessione con l'hub.</span><span class="sxs-lookup"><span data-stu-id="112be-180">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> <span data-ttu-id="112be-181">Questo approccio assicura che il codice stabilisce una connessione prima dell'esecuzione il gestore dell'evento.</span><span class="sxs-lookup"><span data-stu-id="112be-181">This approach ensures that the code establishes a connection before the event handler executes.</span></span>

<span data-ttu-id="112be-182">Il codice avvia la connessione e quindi lo si passa una funzione per gestire l'evento click sulle **inviare** pulsante nella pagina HTML.</span><span class="sxs-lookup"><span data-stu-id="112be-182">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="112be-183">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="112be-183">Additional resources</span></span>

<span data-ttu-id="112be-184">Per altre informazioni su SignalR, vedere le risorse seguenti:</span><span class="sxs-lookup"><span data-stu-id="112be-184">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="112be-185">Progetto di SignalR</span><span class="sxs-lookup"><span data-stu-id="112be-185">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="112be-186">SignalR Github ed esempi</span><span class="sxs-lookup"><span data-stu-id="112be-186">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="112be-187">Wiki di SignalR</span><span class="sxs-lookup"><span data-stu-id="112be-187">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="112be-188">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="112be-188">Next steps</span></span>

<span data-ttu-id="112be-189">In questa esercitazione è:</span><span class="sxs-lookup"><span data-stu-id="112be-189">In this tutorial you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="112be-190">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="112be-190">Set up the project</span></span>
> * <span data-ttu-id="112be-191">È stato eseguito il codice di esempio</span><span class="sxs-lookup"><span data-stu-id="112be-191">Ran the sample</span></span>
> * <span data-ttu-id="112be-192">Esaminare il codice</span><span class="sxs-lookup"><span data-stu-id="112be-192">Examined the code</span></span>

<span data-ttu-id="112be-193">Passare all'articolo successivo per informazioni su come usare SignalR e MVC 5.</span><span class="sxs-lookup"><span data-stu-id="112be-193">Advance to the next article to learn how to use SignalR and MVC 5.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="112be-194">SignalR 2 e MVC 5</span><span class="sxs-lookup"><span data-stu-id="112be-194">SignalR 2 and MVC 5</span></span>](tutorial-getting-started-with-signalr-and-mvc.md)