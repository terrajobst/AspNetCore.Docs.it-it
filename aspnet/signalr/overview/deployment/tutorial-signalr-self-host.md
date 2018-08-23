---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Esercitazione: Hosting indipendente di SignalR | Microsoft Docs'
author: pfletcher
description: Questa esercitazione illustra come creare un server di SignalR 2 self-hosted e come connettersi a esso con un client JavaScript. Versioni del software utilizzate nell'esercitazione V...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 5d7d485357a6c820f11e0135e2ff9479c1965d96
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829588"
---
<a name="tutorial-signalr-self-host"></a><span data-ttu-id="b663e-104">Esercitazione: Hosting indipendente di SignalR</span><span class="sxs-lookup"><span data-stu-id="b663e-104">Tutorial: SignalR Self-Host</span></span>
====================
<span data-ttu-id="b663e-105">da [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="b663e-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[<span data-ttu-id="b663e-106">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="b663e-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> <span data-ttu-id="b663e-107">Questa esercitazione illustra come creare un server di SignalR 2 self-hosted e come connettersi a esso con un client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b663e-107">This tutorial shows how to create a self-hosted SignalR 2 server, and how to connect to it with a JavaScript client.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b663e-108">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="b663e-108">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="b663e-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b663e-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="b663e-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="b663e-110">.NET 4.5</span></span>
> - <span data-ttu-id="b663e-111">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="b663e-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="b663e-112">Uso di Visual Studio 2012 con questa esercitazione</span><span class="sxs-lookup"><span data-stu-id="b663e-112">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="b663e-113">Per usare Visual Studio 2012 con questa esercitazione, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="b663e-113">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="b663e-114">Aggiornamento di [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) alla versione più recente.</span><span class="sxs-lookup"><span data-stu-id="b663e-114">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="b663e-115">Installare il [installazione guidata piattaforma Web](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="b663e-115">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="b663e-116">L'installazione guidata piattaforma Web, cercare e installare **ASP.NET e Web Tools 2013.1 per Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="b663e-116">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="b663e-117">Verrà installato, ad esempio modelli di Visual Studio per le classi di SignalR **Hub**.</span><span class="sxs-lookup"><span data-stu-id="b663e-117">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="b663e-118">Alcuni modelli (ad esempio **classe di avvio OWIN**) non è disponibile; per questo motivo, usare invece un file di classe.</span><span class="sxs-lookup"><span data-stu-id="b663e-118">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="b663e-119">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="b663e-119">Questions and comments</span></span>
> 
> <span data-ttu-id="b663e-120">Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="b663e-120">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="b663e-121">Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="b663e-121">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="b663e-122">Panoramica</span><span class="sxs-lookup"><span data-stu-id="b663e-122">Overview</span></span>

<span data-ttu-id="b663e-123">Un server di SignalR è generalmente ospitato in un'applicazione ASP.NET in IIS, ma può anche essere self-hosted (ad esempio un'applicazione console o il servizio Windows) usando la libreria self-hosting.</span><span class="sxs-lookup"><span data-stu-id="b663e-123">A SignalR server is usually hosted in an ASP.NET application in IIS, but it can also be self-hosted (such as in a console application or Windows service) using the self-host library.</span></span> <span data-ttu-id="b663e-124">Questa libreria, quali tutto SignalR 2, si basa su OWIN ([Open Web Interface for .NET](http://owin.org)).</span><span class="sxs-lookup"><span data-stu-id="b663e-124">This library, like all of SignalR 2, is built on OWIN ([Open Web Interface for .NET](http://owin.org)).</span></span> <span data-ttu-id="b663e-125">OWIN definisce un'astrazione tra i server web .NET e applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="b663e-125">OWIN defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="b663e-126">OWIN consente di disaccoppiare l'applicazione web dal server, che rende ideale per un'applicazione web nel proprio processo, all'esterno di IIS di self-hosting OWIN.</span><span class="sxs-lookup"><span data-stu-id="b663e-126">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>

<span data-ttu-id="b663e-127">I motivi per non effettua l'hosting in IIS includono:</span><span class="sxs-lookup"><span data-stu-id="b663e-127">Reasons for not hosting in IIS include:</span></span>

- <span data-ttu-id="b663e-128">Ambienti in cui IIS non è disponibile o utile, ad esempio una server farm esistente senza IIS.</span><span class="sxs-lookup"><span data-stu-id="b663e-128">Environments where IIS is not available or desirable, such as an existing server farm without IIS.</span></span>
- <span data-ttu-id="b663e-129">L'overhead delle prestazioni di IIS deve essere evitata.</span><span class="sxs-lookup"><span data-stu-id="b663e-129">The performance overhead of IIS needs to be avoided.</span></span>
- <span data-ttu-id="b663e-130">SignalR funzionalità deve essere aggiunto a un'applicazione esistente che viene eseguito in un servizio di Windows, ruolo di lavoro di Azure o altri processi.</span><span class="sxs-lookup"><span data-stu-id="b663e-130">SignalR functionality is to be added to an exising application that runs in a Windows Service, Azure worker role, or other process.</span></span>

<span data-ttu-id="b663e-131">Se una soluzione è in fase di sviluppo come self-hosting per motivi di prestazioni, è consigliabile anche test l'applicazione ospitata in IIS per determinare il miglioramento delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="b663e-131">If a solution is being developed as self-host for performance reasons, it's recommended to also test the application hosted in IIS to determine the performance benefit.</span></span>

<span data-ttu-id="b663e-132">Questa esercitazione include le sezioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="b663e-132">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="b663e-133">Creazione del server</span><span class="sxs-lookup"><span data-stu-id="b663e-133">Creating the server</span></span>](#server)
- [<span data-ttu-id="b663e-134">L'accesso al server con un client JavaScript</span><span class="sxs-lookup"><span data-stu-id="b663e-134">Accessing the server with a JavaScript client</span></span>](#js)

<a id="server"></a>

## <a name="creating-the-server"></a><span data-ttu-id="b663e-135">Creazione del server</span><span class="sxs-lookup"><span data-stu-id="b663e-135">Creating the server</span></span>

<span data-ttu-id="b663e-136">In questa esercitazione si creerà un server in cui è ospitato in un'applicazione console, ma il server può essere ospitato in qualsiasi tipo di processo, ad esempio un servizio di Windows o un ruolo di lavoro di Azure.</span><span class="sxs-lookup"><span data-stu-id="b663e-136">In this tutorial, you'll create a server that's hosted in a console application, but the server can be hosted in any sort of process, such as a Windows service or Azure worker role.</span></span> <span data-ttu-id="b663e-137">Per codice di esempio per l'hosting di un server di SignalR in un servizio di Windows, visitare [Self-Hosting SignalR in un servizio Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span><span class="sxs-lookup"><span data-stu-id="b663e-137">For sample code for hosting a SignalR server in a Windows Service, see [Self-Hosting SignalR in a Windows Service](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span></span>

1. <span data-ttu-id="b663e-138">Aprire Visual Studio 2013 con privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="b663e-138">Open Visual Studio 2013 with administrator privileges.</span></span> <span data-ttu-id="b663e-139">Selezionare **File**, **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="b663e-139">Select **File**, **New Project**.</span></span> <span data-ttu-id="b663e-140">Selezionare **Windows** sotto il **Visual c#** nodo il **modelli** riquadro e selezionare il **applicazione Console** modello.</span><span class="sxs-lookup"><span data-stu-id="b663e-140">Select **Windows** under the **Visual C#** node in the **Templates** pane, and select the **Console Application** template.</span></span> <span data-ttu-id="b663e-141">Denominare il nuovo progetto "SignalRSelfHost" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="b663e-141">Name the new project "SignalRSelfHost" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image1.png)
2. <span data-ttu-id="b663e-142">Aprire la console di gestione pacchetti libreria selezionando **degli strumenti**, **Library Package Manager**, **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="b663e-142">Open the library package manager console by selecting **Tools**, **Library Package Manager**, **Package Manager Console**.</span></span>
3. <span data-ttu-id="b663e-143">Nella console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b663e-143">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    <span data-ttu-id="b663e-144">Questo comando aggiunge le librerie di SignalR 2 al progetto.</span><span class="sxs-lookup"><span data-stu-id="b663e-144">This command adds the SignalR 2 Self-Host libraries to the project.</span></span>
4. <span data-ttu-id="b663e-145">Nella console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b663e-145">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    <span data-ttu-id="b663e-146">Questo comando aggiunge la libreria owin al progetto.</span><span class="sxs-lookup"><span data-stu-id="b663e-146">This command adds the Microsoft.Owin.Cors library to the project.</span></span> <span data-ttu-id="b663e-147">Questa libreria verrà usata per il supporto di domini, che è necessario per le applicazioni che ospitano SignalR e un client della pagina web in domini diversi.</span><span class="sxs-lookup"><span data-stu-id="b663e-147">This library will be used for cross-domain support, which is required for applications that host SignalR and a web page client in different domains.</span></span> <span data-ttu-id="b663e-148">Poiché si sarà ospita il server di SignalR e il client web su porte diverse, ciò significa che tra domini devono essere abilitato per la comunicazione tra questi componenti.</span><span class="sxs-lookup"><span data-stu-id="b663e-148">Since you'll be hosting the SignalR server and the web client on different ports, this means that cross-domain must be enabled for communication between these components.</span></span>
5. <span data-ttu-id="b663e-149">Sostituire il contenuto di Program.cs con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="b663e-149">Replace the contents of Program.cs with the following code.</span></span>

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    <span data-ttu-id="b663e-150">Il codice riportato sopra include le tre classi seguenti:</span><span class="sxs-lookup"><span data-stu-id="b663e-150">The above code includes three classes:</span></span>

    - <span data-ttu-id="b663e-151">**Programma**, tra cui la **Main** metodo che definisce il percorso principale di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="b663e-151">**Program**, including the **Main** method defining the primary path of execution.</span></span> <span data-ttu-id="b663e-152">In questo metodo, un'applicazione web di tipo **avvio** viene avviato all'URL specificato (`http://localhost:8080`).</span><span class="sxs-lookup"><span data-stu-id="b663e-152">In this method, a web application of type **Startup** is started at the specified URL (`http://localhost:8080`).</span></span> <span data-ttu-id="b663e-153">Se sicurezza è obbligatorio per l'endpoint, è possibile implementare SSL.</span><span class="sxs-lookup"><span data-stu-id="b663e-153">If security is required on the endpoint, SSL can be implemented.</span></span> <span data-ttu-id="b663e-154">Visualizzare [procedura: configurare una porta con un certificato SSL](https://msdn.microsoft.com/library/ms733791.aspx) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="b663e-154">See [How to: Configure a Port with an SSL Certificate](https://msdn.microsoft.com/library/ms733791.aspx) for more information.</span></span>
    - <span data-ttu-id="b663e-155">**Avvio**, la classe che contiene la configurazione per il server di SignalR (l'unica configurazione di questa esercitazione viene usato è la chiamata a `UseCors`) e la chiamata a `MapSignalR`, che consente di creare le route per gli oggetti dell'Hub nel progetto.</span><span class="sxs-lookup"><span data-stu-id="b663e-155">**Startup**, the class containing the configuration for the SignalR server (the only configuration this tutorial uses is the call to `UseCors`), and the call to `MapSignalR`, which creates routes for any Hub objects in the project.</span></span>
    - <span data-ttu-id="b663e-156">**MyHub**, la classe SignalR Hub che l'applicazione dovrà fornire ai client.</span><span class="sxs-lookup"><span data-stu-id="b663e-156">**MyHub**, the SignalR Hub class that the application will provide to clients.</span></span> <span data-ttu-id="b663e-157">Questa classe ha un solo metodo, **inviare**, che i client chiameranno per trasmettere un messaggio a tutti gli altri client connessi.</span><span class="sxs-lookup"><span data-stu-id="b663e-157">This class has a single method, **Send**, that clients will call to broadcast a message to all other connected clients.</span></span>
6. <span data-ttu-id="b663e-158">Compilare l'applicazione ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="b663e-158">Compile and run the application.</span></span> <span data-ttu-id="b663e-159">Nella finestra della console dovrebbe visualizzare l'indirizzo a cui il server è in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="b663e-159">The address that the server is running should show in a console window.</span></span>

    ![](tutorial-signalr-self-host/_static/image2.png)
7. <span data-ttu-id="b663e-160">Se l'esecuzione ha esito negativo con l'eccezione `System.Reflection.TargetInvocationException was unhandled`, sarà necessario riavviare Visual Studio con privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="b663e-160">If execution fails with the exception `System.Reflection.TargetInvocationException was unhandled`, you will need to restart Visual Studio with administrator privileges.</span></span>
8. <span data-ttu-id="b663e-161">Arrestare l'applicazione prima di procedere alla sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="b663e-161">Stop the application before proceeding to the next section.</span></span>

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a><span data-ttu-id="b663e-162">L'accesso al server con un client JavaScript</span><span class="sxs-lookup"><span data-stu-id="b663e-162">Accessing the server with a JavaScript client</span></span>

<span data-ttu-id="b663e-163">In questa sezione si userà lo stesso client JavaScript dal [esercitazione introduttiva](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="b663e-163">In this section, you'll use the same JavaScript client from the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="b663e-164">Ci assicureremo solo una modifica al client, che consiste nel definire in modo esplicito l'URL dell'hub.</span><span class="sxs-lookup"><span data-stu-id="b663e-164">We'll only make one modification to the client, which is to explicitly define the hub URL.</span></span> <span data-ttu-id="b663e-165">Con un'applicazione self-hosted, il server potrebbe non corrispondere esattamente allo stesso indirizzo come URL della connessione (a causa di un proxy inverso e i bilanciamenti del carico), pertanto, l'URL deve essere definito in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="b663e-165">With a self-hosted application, the server may not necessarily be at the same address as the connection URL (due to reverse proxies and load balancers), so the URL needs to be defined explicitly.</span></span>

1. <span data-ttu-id="b663e-166">Nelle **Esplora soluzioni**, fare doppio clic sulla soluzione e selezionare **Add**, **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="b663e-166">In **Solution Explorer**, right-click on the solution and select **Add**, **New Project**.</span></span> <span data-ttu-id="b663e-167">Selezionare il **Web** nodo e selezionare il **applicazione Web ASP.NET** modello.</span><span class="sxs-lookup"><span data-stu-id="b663e-167">Select the **Web** node, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="b663e-168">Denominare il progetto "JavascriptClient" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="b663e-168">Name the project "JavascriptClient" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image3.png)
2. <span data-ttu-id="b663e-169">Selezionare il **vuoto** modello e lasciare le altre opzioni deselezionate.</span><span class="sxs-lookup"><span data-stu-id="b663e-169">Select the **Empty** template, and leave the remaining options unselected.</span></span> <span data-ttu-id="b663e-170">Selezionare **Crea progetto**.</span><span class="sxs-lookup"><span data-stu-id="b663e-170">Select **Create Project**.</span></span>

    ![](tutorial-signalr-self-host/_static/image4.png)
3. <span data-ttu-id="b663e-171">Nella console di gestione pacchetti, selezionare il progetto "JavascriptClient" nel **progetto predefinito** elenco a discesa ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b663e-171">In the package manager console, select the "JavascriptClient" project in the **Default project** drop-down, and execute the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    <span data-ttu-id="b663e-172">Questo comando Installa le librerie di SignalR e JQuery che saranno necessari nel client.</span><span class="sxs-lookup"><span data-stu-id="b663e-172">This command installs the SignalR and JQuery libraries that you'll need in the client.</span></span>
4. <span data-ttu-id="b663e-173">Pulsante destro del mouse sul progetto, quindi scegliere **Add**, **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="b663e-173">Right-click on your project and select **Add**, **New Item**.</span></span> <span data-ttu-id="b663e-174">Selezionare il **Web** nodo e selezionare pagina HTML.</span><span class="sxs-lookup"><span data-stu-id="b663e-174">Select the **Web** node, and select HTML Page.</span></span> <span data-ttu-id="b663e-175">Denominare la pagina **default. HTML**.</span><span class="sxs-lookup"><span data-stu-id="b663e-175">Name the page **Default.html**.</span></span>

    ![](tutorial-signalr-self-host/_static/image5.png)
5. <span data-ttu-id="b663e-176">Sostituire il contenuto della nuova pagina HTML con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="b663e-176">Replace the contents of the new HTML page with the following code.</span></span> <span data-ttu-id="b663e-177">Verificare che i riferimenti a script corrispondano gli script nella cartella Scripts del progetto.</span><span class="sxs-lookup"><span data-stu-id="b663e-177">Verify that the script references here match the scripts in the Scripts folder of the project.</span></span>

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    <span data-ttu-id="b663e-178">Il codice seguente (evidenziato nell'esempio di codice precedente) è l'aggiunta che sono state apportate al client usato nell'esercitazione di introduzione all'uso (oltre ad aggiornare il codice a SignalR 2 versione beta).</span><span class="sxs-lookup"><span data-stu-id="b663e-178">The following code (highlighted in the code sample above) is the addition that you've made to the client used in the Getting Stared tutorial (in addition to upgrading the code to SignalR version 2 beta).</span></span> <span data-ttu-id="b663e-179">Questa riga di codice imposta in modo esplicito l'URL di connessione di base per SignalR sul server.</span><span class="sxs-lookup"><span data-stu-id="b663e-179">This line of code explicitly sets the base connection URL for SignalR on the server.</span></span>

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. <span data-ttu-id="b663e-180">Pulsante destro del mouse sulla soluzione e selezionare **Imposta progetti di avvio...** . Selezionare il **progetti di avvio multipli** pulsante di opzione e impostare entrambi i progetti **azione** al **avviare**.</span><span class="sxs-lookup"><span data-stu-id="b663e-180">Right-click on the solution, and select **Set Startup Projects...**. Select the **Multiple startup projects** radio button, and set both projects' **Action** to **Start**.</span></span>

    ![](tutorial-signalr-self-host/_static/image6.png)
7. <span data-ttu-id="b663e-181">Fare clic su "Default. HTML" e selezionare **imposta come pagina iniziale**.</span><span class="sxs-lookup"><span data-stu-id="b663e-181">Right-click on "Default.html" and select **Set As Start Page**.</span></span>
8. <span data-ttu-id="b663e-182">Eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b663e-182">Run the application.</span></span> <span data-ttu-id="b663e-183">Verrà avviata la pagina e il server.</span><span class="sxs-lookup"><span data-stu-id="b663e-183">The server and page will launch.</span></span> <span data-ttu-id="b663e-184">Potrebbe essere necessario ricaricare la pagina web (o selezionare **continuazione** nel debugger) se la pagina viene caricata prima che il server è stato avviato.</span><span class="sxs-lookup"><span data-stu-id="b663e-184">You may need to reload the web page (or select **Continue** in the debugger) if the page loads before the server is started.</span></span>
9. <span data-ttu-id="b663e-185">Nel browser, fornire un nome utente quando richiesto.</span><span class="sxs-lookup"><span data-stu-id="b663e-185">In the browser, provide a username when prompted.</span></span> <span data-ttu-id="b663e-186">Copiare l'URL della pagina in un'altra scheda del browser o finestra e fornire un nome utente diverso.</span><span class="sxs-lookup"><span data-stu-id="b663e-186">Copy the page's URL into another browser tab or window and provide a different username.</span></span> <span data-ttu-id="b663e-187">Sarà in grado di inviare messaggi dal riquadro uno visualizzatore a altro, come nell'esercitazione introduttiva.</span><span class="sxs-lookup"><span data-stu-id="b663e-187">You will be able to send messages from one browser pane to the other, as in the Getting Started tutorial.</span></span>
