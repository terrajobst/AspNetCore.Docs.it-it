---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Esercitazione: SignalR indipendente | Documenti Microsoft'
author: pfletcher
description: Questa esercitazione viene illustrato come creare un server di SignalR 2 indipendente e come connettersi con un client JavaScript. Versioni del software utilizzate nell'esercitazione V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: d38e6fbc3407e4beca6942bbdefcaa8258ebc5ad
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036778"
---
<a name="tutorial-signalr-self-host"></a><span data-ttu-id="d2549-104">Esercitazione: SignalR Host indipendente</span><span class="sxs-lookup"><span data-stu-id="d2549-104">Tutorial: SignalR Self-Host</span></span>
====================
<span data-ttu-id="d2549-105">da [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="d2549-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="d2549-106">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="d2549-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> <span data-ttu-id="d2549-107">Questa esercitazione viene illustrato come creare un server di SignalR 2 indipendente e come connettersi con un client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d2549-107">This tutorial shows how to create a self-hosted SignalR 2 server, and how to connect to it with a JavaScript client.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d2549-108">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="d2549-108">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="d2549-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d2549-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="d2549-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="d2549-110">.NET 4.5</span></span>
> - <span data-ttu-id="d2549-111">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="d2549-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="d2549-112">Utilizzo di Visual Studio 2012 con questa esercitazione</span><span class="sxs-lookup"><span data-stu-id="d2549-112">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="d2549-113">Per utilizzare Visual Studio 2012 con questa esercitazione, effettuare le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="d2549-113">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="d2549-114">Aggiornamento del [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) alla versione più recente.</span><span class="sxs-lookup"><span data-stu-id="d2549-114">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="d2549-115">Installare il [installazione guidata piattaforma Web](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="d2549-115">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="d2549-116">L'installazione guidata piattaforma Web, cercare e installare **ASP.NET e 2013.1 strumenti Web per Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="d2549-116">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="d2549-117">Verrà installato come modelli di Visual Studio per le classi di SignalR **Hub**.</span><span class="sxs-lookup"><span data-stu-id="d2549-117">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="d2549-118">Alcuni modelli (ad esempio **classe di avvio di OWIN**) non saranno disponibili per tali file, usare un file di classe.</span><span class="sxs-lookup"><span data-stu-id="d2549-118">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="d2549-119">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="d2549-119">Questions and comments</span></span>
> 
> <span data-ttu-id="d2549-120">Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="d2549-120">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="d2549-121">In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="d2549-121">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="d2549-122">Panoramica</span><span class="sxs-lookup"><span data-stu-id="d2549-122">Overview</span></span>

<span data-ttu-id="d2549-123">Un server di SignalR viene solitamente ospitato in un'applicazione ASP.NET in IIS, ma può anche essere indipendente (ad esempio un'applicazione console o il servizio Windows) utilizzando la libreria di host indipendente.</span><span class="sxs-lookup"><span data-stu-id="d2549-123">A SignalR server is usually hosted in an ASP.NET application in IIS, but it can also be self-hosted (such as in a console application or Windows service) using the self-host library.</span></span> <span data-ttu-id="d2549-124">Questa libreria, come tutte SignalR 2, si basa su OWIN ([aprire interfaccia Web per .NET](http://owin.org)).</span><span class="sxs-lookup"><span data-stu-id="d2549-124">This library, like all of SignalR 2, is built on OWIN ([Open Web Interface for .NET](http://owin.org)).</span></span> <span data-ttu-id="d2549-125">OWIN definisce un'astrazione tra i server web .NET e applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="d2549-125">OWIN defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="d2549-126">OWIN disaccoppia l'applicazione web dal server, che rende ideale per l'hosting automatico di un'applicazione web in un processo personalizzato, all'esterno di IIS OWIN.</span><span class="sxs-lookup"><span data-stu-id="d2549-126">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>

<span data-ttu-id="d2549-127">I motivi per cui non è ospitato in IIS includono:</span><span class="sxs-lookup"><span data-stu-id="d2549-127">Reasons for not hosting in IIS include:</span></span>

- <span data-ttu-id="d2549-128">Ambienti in cui IIS non è disponibile o, ad esempio una server farm esistente senza IIS.</span><span class="sxs-lookup"><span data-stu-id="d2549-128">Environments where IIS is not available or desirable, such as an existing server farm without IIS.</span></span>
- <span data-ttu-id="d2549-129">L'overhead delle prestazioni di IIS deve essere evitato.</span><span class="sxs-lookup"><span data-stu-id="d2549-129">The performance overhead of IIS needs to be avoided.</span></span>
- <span data-ttu-id="d2549-130">Funzionalità di SignalR è da aggiungere a un'applicazione esistente che viene eseguito in un servizio Windows, il ruolo di lavoro di Azure o un altro processo.</span><span class="sxs-lookup"><span data-stu-id="d2549-130">SignalR functionality is to be added to an exising application that runs in a Windows Service, Azure worker role, or other process.</span></span>

<span data-ttu-id="d2549-131">Se una soluzione di sviluppo come servizio indipendente per motivi di prestazioni, si consiglia di test anche l'applicazione ospitata in IIS per determinare il miglioramento delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="d2549-131">If a solution is being developed as self-host for performance reasons, it's recommended to also test the application hosted in IIS to determine the performance benefit.</span></span>

<span data-ttu-id="d2549-132">In questa esercitazione include le sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="d2549-132">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="d2549-133">La creazione del server</span><span class="sxs-lookup"><span data-stu-id="d2549-133">Creating the server</span></span>](#server)
- [<span data-ttu-id="d2549-134">L'accesso al server con un client JavaScript</span><span class="sxs-lookup"><span data-stu-id="d2549-134">Accessing the server with a JavaScript client</span></span>](#js)

<a id="server"></a>

## <a name="creating-the-server"></a><span data-ttu-id="d2549-135">La creazione del server</span><span class="sxs-lookup"><span data-stu-id="d2549-135">Creating the server</span></span>

<span data-ttu-id="d2549-136">In questa esercitazione si creerà un server in cui è ospitato in un'applicazione console, ma il server può essere ospitato in un qualsiasi tipo di processo, ad esempio un servizio Windows o un ruolo di lavoro di Azure.</span><span class="sxs-lookup"><span data-stu-id="d2549-136">In this tutorial, you'll create a server that's hosted in a console application, but the server can be hosted in any sort of process, such as a Windows service or Azure worker role.</span></span> <span data-ttu-id="d2549-137">Per il codice di esempio per l'hosting di un server di SignalR in un servizio Windows, vedere [Self-Hosting SignalR in un servizio Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span><span class="sxs-lookup"><span data-stu-id="d2549-137">For sample code for hosting a SignalR server in a Windows Service, see [Self-Hosting SignalR in a Windows Service](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span></span>

1. <span data-ttu-id="d2549-138">Aprire Visual Studio 2013 con privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="d2549-138">Open Visual Studio 2013 with administrator privileges.</span></span> <span data-ttu-id="d2549-139">Selezionare **File**, **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="d2549-139">Select **File**, **New Project**.</span></span> <span data-ttu-id="d2549-140">Selezionare **Windows** sotto il **Visual c#** nodo il **modelli** riquadro e selezionare il **applicazione Console** modello.</span><span class="sxs-lookup"><span data-stu-id="d2549-140">Select **Windows** under the **Visual C#** node in the **Templates** pane, and select the **Console Application** template.</span></span> <span data-ttu-id="d2549-141">Denominare il nuovo progetto "SignalRSelfHost" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="d2549-141">Name the new project "SignalRSelfHost" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image1.png)
2. <span data-ttu-id="d2549-142">Aprire la console di gestione pacchetti libreria selezionando **strumenti**, **Gestione pacchetti libreria**, **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="d2549-142">Open the library package manager console by selecting **Tools**, **Library Package Manager**, **Package Manager Console**.</span></span>
3. <span data-ttu-id="d2549-143">Nella console di gestione di pacchetto, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="d2549-143">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    <span data-ttu-id="d2549-144">Questo comando aggiunge le librerie di SignalR 2 Self-Host al progetto.</span><span class="sxs-lookup"><span data-stu-id="d2549-144">This command adds the SignalR 2 Self-Host libraries to the project.</span></span>
4. <span data-ttu-id="d2549-145">Nella console di gestione di pacchetto, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="d2549-145">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    <span data-ttu-id="d2549-146">Questo comando aggiunge la libreria owin al progetto.</span><span class="sxs-lookup"><span data-stu-id="d2549-146">This command adds the Microsoft.Owin.Cors library to the project.</span></span> <span data-ttu-id="d2549-147">La libreria da utilizzare per il supporto di domini, è necessario per le applicazioni che ospitano SignalR e un client di pagina web in domini diversi.</span><span class="sxs-lookup"><span data-stu-id="d2549-147">This library will be used for cross-domain support, which is required for applications that host SignalR and a web page client in different domains.</span></span> <span data-ttu-id="d2549-148">Poiché si sarà ospita il server di SignalR e il client web su porte diverse, ciò significa che tra domini devono essere abilitato per la comunicazione tra questi componenti.</span><span class="sxs-lookup"><span data-stu-id="d2549-148">Since you'll be hosting the SignalR server and the web client on different ports, this means that cross-domain must be enabled for communication between these components.</span></span>
5. <span data-ttu-id="d2549-149">Sostituire il contenuto di Program.cs con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="d2549-149">Replace the contents of Program.cs with the following code.</span></span>

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    <span data-ttu-id="d2549-150">Il codice sopra riportato include tre classi:</span><span class="sxs-lookup"><span data-stu-id="d2549-150">The above code includes three classes:</span></span>

    - <span data-ttu-id="d2549-151">**Programma**, tra cui la **Main** metodo che definisce il percorso principale di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d2549-151">**Program**, including the **Main** method defining the primary path of execution.</span></span> <span data-ttu-id="d2549-152">In questo metodo, un'applicazione web di tipo **avvio** viene avviato all'URL specificato (`http://localhost:8080`).</span><span class="sxs-lookup"><span data-stu-id="d2549-152">In this method, a web application of type **Startup** is started at the specified URL (`http://localhost:8080`).</span></span> <span data-ttu-id="d2549-153">Se è richiesta la protezione per l'endpoint, è possibile implementare SSL.</span><span class="sxs-lookup"><span data-stu-id="d2549-153">If security is required on the endpoint, SSL can be implemented.</span></span> <span data-ttu-id="d2549-154">Vedere [procedura: configurare una porta con un certificato SSL](https://msdn.microsoft.com/library/ms733791.aspx) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="d2549-154">See [How to: Configure a Port with an SSL Certificate](https://msdn.microsoft.com/library/ms733791.aspx) for more information.</span></span>
    - <span data-ttu-id="d2549-155">**Avvio**, la classe che contiene la configurazione per il server di SignalR (la sola configurazione di questa esercitazione è la chiamata a `UseCors`) e la chiamata a `MapSignalR`, che consente di creare le route per tutti gli oggetti Hub nel progetto.</span><span class="sxs-lookup"><span data-stu-id="d2549-155">**Startup**, the class containing the configuration for the SignalR server (the only configuration this tutorial uses is the call to `UseCors`), and the call to `MapSignalR`, which creates routes for any Hub objects in the project.</span></span>
    - <span data-ttu-id="d2549-156">**MyHub**, la classe SignalR Hub che l'applicazione fornisce ai client.</span><span class="sxs-lookup"><span data-stu-id="d2549-156">**MyHub**, the SignalR Hub class that the application will provide to clients.</span></span> <span data-ttu-id="d2549-157">Questa classe contiene un solo metodo, **inviare**, che i client chiameranno per trasmettere un messaggio a tutti gli altri client connessi.</span><span class="sxs-lookup"><span data-stu-id="d2549-157">This class has a single method, **Send**, that clients will call to broadcast a message to all other connected clients.</span></span>
6. <span data-ttu-id="d2549-158">Compilare l'applicazione ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="d2549-158">Compile and run the application.</span></span> <span data-ttu-id="d2549-159">L'indirizzo a cui è in esecuzione il server deve visualizzare in una finestra della console.</span><span class="sxs-lookup"><span data-stu-id="d2549-159">The address that the server is running should show in a console window.</span></span>

    ![](tutorial-signalr-self-host/_static/image2.png)
7. <span data-ttu-id="d2549-160">Se si verifica un errore di esecuzione con l'eccezione `System.Reflection.TargetInvocationException was unhandled`, è necessario riavviare Visual Studio con privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="d2549-160">If execution fails with the exception `System.Reflection.TargetInvocationException was unhandled`, you will need to restart Visual Studio with administrator privileges.</span></span>
8. <span data-ttu-id="d2549-161">Arrestare l'applicazione prima di procedere alla sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="d2549-161">Stop the application before proceeding to the next section.</span></span>

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a><span data-ttu-id="d2549-162">L'accesso al server con un client JavaScript</span><span class="sxs-lookup"><span data-stu-id="d2549-162">Accessing the server with a JavaScript client</span></span>

<span data-ttu-id="d2549-163">In questa sezione, si utilizzerà lo stesso client JavaScript dal [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="d2549-163">In this section, you'll use the same JavaScript client from the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="d2549-164">Verrà effettuata solo una modifica al client, che consiste nel definire in modo esplicito l'URL di hub.</span><span class="sxs-lookup"><span data-stu-id="d2549-164">We'll only make one modification to the client, which is to explicitly define the hub URL.</span></span> <span data-ttu-id="d2549-165">Con un'applicazione indipendente, il server potrebbe non necessariamente essere allo stesso indirizzo come l'URL di connessione (a causa di proxy inverso e i servizi di bilanciamento del carico), pertanto l'URL deve essere definito in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="d2549-165">With a self-hosted application, the server may not necessarily be at the same address as the connection URL (due to reverse proxies and load balancers), so the URL needs to be defined explicitly.</span></span>

1. <span data-ttu-id="d2549-166">In **Esplora**del mouse sulla soluzione e scegliere **Aggiungi**, **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="d2549-166">In **Solution Explorer**, right-click on the solution and select **Add**, **New Project**.</span></span> <span data-ttu-id="d2549-167">Selezionare il **Web** nodo e selezionare il **applicazione Web ASP.NET** modello.</span><span class="sxs-lookup"><span data-stu-id="d2549-167">Select the **Web** node, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="d2549-168">Denominare il progetto "JavascriptClient" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="d2549-168">Name the project "JavascriptClient" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image3.png)
2. <span data-ttu-id="d2549-169">Selezionare il **vuoto** modello e lasciare deselezionate le opzioni rimanenti.</span><span class="sxs-lookup"><span data-stu-id="d2549-169">Select the **Empty** template, and leave the remaining options unselected.</span></span> <span data-ttu-id="d2549-170">Selezionare **creare progetto**.</span><span class="sxs-lookup"><span data-stu-id="d2549-170">Select **Create Project**.</span></span>

    ![](tutorial-signalr-self-host/_static/image4.png)
3. <span data-ttu-id="d2549-171">Nella console di gestione di pacchetto, selezionare il progetto "JavascriptClient" nel **progetto predefinito** elenco a discesa ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="d2549-171">In the package manager console, select the "JavascriptClient" project in the **Default project** drop-down, and execute the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    <span data-ttu-id="d2549-172">Questo comando Installa le librerie di SignalR e JQuery è necessario nel client.</span><span class="sxs-lookup"><span data-stu-id="d2549-172">This command installs the SignalR and JQuery libraries that you'll need in the client.</span></span>
4. <span data-ttu-id="d2549-173">Pulsante destro del mouse sul progetto e selezionare **Aggiungi**, **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="d2549-173">Right-click on your project and select **Add**, **New Item**.</span></span> <span data-ttu-id="d2549-174">Selezionare il **Web** nodo e selezionare una pagina HTML.</span><span class="sxs-lookup"><span data-stu-id="d2549-174">Select the **Web** node, and select HTML Page.</span></span> <span data-ttu-id="d2549-175">Denominare la pagina **Default.html**.</span><span class="sxs-lookup"><span data-stu-id="d2549-175">Name the page **Default.html**.</span></span>

    ![](tutorial-signalr-self-host/_static/image5.png)
5. <span data-ttu-id="d2549-176">Sostituire il contenuto della nuova pagina HTML con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="d2549-176">Replace the contents of the new HTML page with the following code.</span></span> <span data-ttu-id="d2549-177">Verificare che i riferimenti a script corrispondano gli script nella cartella script del progetto.</span><span class="sxs-lookup"><span data-stu-id="d2549-177">Verify that the script references here match the scripts in the Scripts folder of the project.</span></span>

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    <span data-ttu-id="d2549-178">Il codice seguente (evidenziato nell'esempio di codice precedente) è l'aggiunta che sono state apportate al client utilizzato nell'esercitazione recupero scoraggiato (oltre ad aggiornare il codice per SignalR versione 2 beta).</span><span class="sxs-lookup"><span data-stu-id="d2549-178">The following code (highlighted in the code sample above) is the addition that you've made to the client used in the Getting Stared tutorial (in addition to upgrading the code to SignalR version 2 beta).</span></span> <span data-ttu-id="d2549-179">Questa riga di codice imposta in modo esplicito l'URL di connessione di base per SignalR sul server.</span><span class="sxs-lookup"><span data-stu-id="d2549-179">This line of code explicitly sets the base connection URL for SignalR on the server.</span></span>

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. <span data-ttu-id="d2549-180">Pulsante destro del mouse sulla soluzione e selezionare **Imposta progetti di avvio...** . Selezionare il **più progetti di avvio** pulsante di opzione e impostare entrambi i progetti **azione** a **avviare**.</span><span class="sxs-lookup"><span data-stu-id="d2549-180">Right-click on the solution, and select **Set Startup Projects...**. Select the **Multiple startup projects** radio button, and set both projects' **Action** to **Start**.</span></span>

    ![](tutorial-signalr-self-host/_static/image6.png)
7. <span data-ttu-id="d2549-181">Fare clic su "Default" e selezionare **imposta come pagina iniziale**.</span><span class="sxs-lookup"><span data-stu-id="d2549-181">Right-click on "Default.html" and select **Set As Start Page**.</span></span>
8. <span data-ttu-id="d2549-182">Eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d2549-182">Run the application.</span></span> <span data-ttu-id="d2549-183">Verrà avviato il server e pagina.</span><span class="sxs-lookup"><span data-stu-id="d2549-183">The server and page will launch.</span></span> <span data-ttu-id="d2549-184">Potrebbe essere necessario ricaricare la pagina web (o selezionare **continua** nel debugger) se il caricamento della pagina prima dell'avvio del server.</span><span class="sxs-lookup"><span data-stu-id="d2549-184">You may need to reload the web page (or select **Continue** in the debugger) if the page loads before the server is started.</span></span>
9. <span data-ttu-id="d2549-185">Nel browser, fornire un nome utente quando richiesto.</span><span class="sxs-lookup"><span data-stu-id="d2549-185">In the browser, provide a username when prompted.</span></span> <span data-ttu-id="d2549-186">Copiare l'URL della pagina in un'altra scheda del browser o una finestra e fornire un nome utente diverso.</span><span class="sxs-lookup"><span data-stu-id="d2549-186">Copy the page's URL into another browser tab or window and provide a different username.</span></span> <span data-ttu-id="d2549-187">Sarà in grado di inviare messaggi dal riquadro uno visualizzatore a altro, come illustrato nell'esercitazione Introduzione.</span><span class="sxs-lookup"><span data-stu-id="d2549-187">You will be able to send messages from one browser pane to the other, as in the Getting Started tutorial.</span></span>
