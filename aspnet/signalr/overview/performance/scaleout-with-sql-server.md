---
uid: signalr/overview/performance/scaleout-with-sql-server
title: Scalabilità orizzontale SignalR con SQL Server | Documenti Microsoft
author: MikeWasson
description: Versioni del software utilizzato in questo argomento di Visual Studio 2013 .NET 4.5 SignalR le versioni precedenti di versione 2 di questo argomento per informazioni sulle versioni precedenti di...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: b3189c36fc076333c0c6007bd039b12e03d63bc8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874369"
---
<a name="signalr-scaleout-with-sql-server"></a><span data-ttu-id="693ef-103">Scalabilità orizzontale SignalR con SQL Server</span><span class="sxs-lookup"><span data-stu-id="693ef-103">SignalR Scaleout with SQL Server</span></span>
====================
<span data-ttu-id="693ef-104">dal [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="693ef-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="693ef-105">Versioni del software utilizzate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="693ef-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="693ef-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="693ef-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="693ef-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="693ef-107">.NET 4.5</span></span>
> - <span data-ttu-id="693ef-108">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="693ef-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="693ef-109">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="693ef-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="693ef-110">Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="693ef-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="693ef-111">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="693ef-111">Questions and comments</span></span>
> 
> <span data-ttu-id="693ef-112">Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="693ef-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="693ef-113">In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="693ef-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="693ef-114">In questa esercitazione si utilizzerà SQL Server per distribuire i messaggi tra un'applicazione di SignalR distribuito in due istanze separate di IIS.</span><span class="sxs-lookup"><span data-stu-id="693ef-114">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="693ef-115">È anche possibile eseguire questa esercitazione in un computer singolo test, ma per ottenere l'effetto completo, è necessario distribuire l'applicazione di SignalR per due o più server.</span><span class="sxs-lookup"><span data-stu-id="693ef-115">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="693ef-116">È inoltre necessario installare SQL Server in uno dei server o in un server dedicato.</span><span class="sxs-lookup"><span data-stu-id="693ef-116">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="693ef-117">Un'altra opzione consiste nell'eseguire l'esercitazione con macchine virtuali in Azure.</span><span class="sxs-lookup"><span data-stu-id="693ef-117">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="693ef-118">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="693ef-118">Prerequisites</span></span>

<span data-ttu-id="693ef-119">Microsoft SQL Server 2005 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="693ef-119">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="693ef-120">Backplane supporta le edizioni di desktop e server di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="693ef-120">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="693ef-121">Non supporta Database SQL Azure o SQL Server Compact Edition.</span><span class="sxs-lookup"><span data-stu-id="693ef-121">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="693ef-122">(Se l'applicazione è ospitata in Azure, è consigliabile backplane Bus di servizio invece.)</span><span class="sxs-lookup"><span data-stu-id="693ef-122">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="693ef-123">Panoramica</span><span class="sxs-lookup"><span data-stu-id="693ef-123">Overview</span></span>

<span data-ttu-id="693ef-124">Prima di passare all'esercitazione dettagliata, ecco una rapida panoramica delle azioni da eseguire.</span><span class="sxs-lookup"><span data-stu-id="693ef-124">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="693ef-125">Creare un nuovo database vuoto.</span><span class="sxs-lookup"><span data-stu-id="693ef-125">Create a new empty database.</span></span> <span data-ttu-id="693ef-126">Backplane creerà le tabelle necessarie nel database.</span><span class="sxs-lookup"><span data-stu-id="693ef-126">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="693ef-127">Aggiungere questi pacchetti NuGet per l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="693ef-127">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="693ef-128">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="693ef-128">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="693ef-129">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="693ef-129">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="693ef-130">Creare un'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="693ef-130">Create a SignalR application.</span></span>
4. <span data-ttu-id="693ef-131">Aggiungere il codice seguente al Startup.cs configurare backplane:</span><span class="sxs-lookup"><span data-stu-id="693ef-131">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   <span data-ttu-id="693ef-132">Questo codice consente di configurare backplane con i valori predefiniti per [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) e [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="693ef-132">This code configures the backplane with the default values for [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="693ef-133">Per informazioni sulla modifica di questi valori, vedere [delle prestazioni di SignalR: metriche di scalabilità orizzontale](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="693ef-133">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span> 

## <a name="configure-the-database"></a><span data-ttu-id="693ef-134">Configurare il Database</span><span class="sxs-lookup"><span data-stu-id="693ef-134">Configure the Database</span></span>

<span data-ttu-id="693ef-135">Decidere se l'applicazione utilizzerà l'autenticazione di Windows o autenticazione di SQL Server per accedere al database.</span><span class="sxs-lookup"><span data-stu-id="693ef-135">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="693ef-136">Verificare in ogni caso, che l'utente del database disponga delle autorizzazioni per accedere, creare gli schemi e creare tabelle.</span><span class="sxs-lookup"><span data-stu-id="693ef-136">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="693ef-137">Creare un nuovo database per backplane da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="693ef-137">Create a new database for the backplane to use.</span></span> <span data-ttu-id="693ef-138">È possibile assegnare il database di qualsiasi nome.</span><span class="sxs-lookup"><span data-stu-id="693ef-138">You can give the database any name.</span></span> <span data-ttu-id="693ef-139">Non è necessario creare tutte le tabelle nel database. backplane creerà le tabelle necessarie.</span><span class="sxs-lookup"><span data-stu-id="693ef-139">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="693ef-140">Abilitare Service Broker</span><span class="sxs-lookup"><span data-stu-id="693ef-140">Enable Service Broker</span></span>

<span data-ttu-id="693ef-141">È consigliabile abilitare Service Broker per il database backplane.</span><span class="sxs-lookup"><span data-stu-id="693ef-141">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="693ef-142">Service Broker fornisce supporto nativo per la messaggistica e Accodamento in SQL Server, che consente di ricevere gli aggiornamenti in modo più efficiente backplane.</span><span class="sxs-lookup"><span data-stu-id="693ef-142">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="693ef-143">(Tuttavia, backplane funziona anche senza Service Broker.)</span><span class="sxs-lookup"><span data-stu-id="693ef-143">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="693ef-144">Per controllare se Service Broker è abilitato, eseguire una query di **è\_broker\_abilitato** colonna il **Sys. Databases** vista del catalogo.</span><span class="sxs-lookup"><span data-stu-id="693ef-144">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="693ef-145">Per abilitare Service Broker, usare la seguente query SQL:</span><span class="sxs-lookup"><span data-stu-id="693ef-145">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="693ef-146">Se questa query viene visualizzato per verificare un deadlock, assicurarsi che non sono presenti applicazioni connesse al database.</span><span class="sxs-lookup"><span data-stu-id="693ef-146">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>


<span data-ttu-id="693ef-147">Se è stata abilitata la traccia, le tracce verranno visualizzata anche se Service Broker è abilitato.</span><span class="sxs-lookup"><span data-stu-id="693ef-147">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="693ef-148">Creare un'applicazione di SignalR</span><span class="sxs-lookup"><span data-stu-id="693ef-148">Create a SignalR Application</span></span>

<span data-ttu-id="693ef-149">Creare un'applicazione di SignalR una di queste esercitazioni:</span><span class="sxs-lookup"><span data-stu-id="693ef-149">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="693ef-150">Introduzione a SignalR 2.0</span><span class="sxs-lookup"><span data-stu-id="693ef-150">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="693ef-151">Introduzione a SignalR 2.0 e MVC 5</span><span class="sxs-lookup"><span data-stu-id="693ef-151">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="693ef-152">Successivamente, verrà modificata l'applicazione di chat per il supporto di scalabilità orizzontale con SQL Server.</span><span class="sxs-lookup"><span data-stu-id="693ef-152">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="693ef-153">In primo luogo, aggiungere il pacchetto SignalR.SqlServer NuGet al progetto.</span><span class="sxs-lookup"><span data-stu-id="693ef-153">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="693ef-154">In Visual Studio, dal **strumenti** dal menu **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="693ef-154">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="693ef-155">Nella finestra della Console di gestione pacchetti, immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="693ef-155">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="693ef-156">Successivamente, aprire il file Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="693ef-156">Next, open the Startup.cs file.</span></span> <span data-ttu-id="693ef-157">Aggiungere il codice seguente per il **configura** metodo:</span><span class="sxs-lookup"><span data-stu-id="693ef-157">Add the following code to the **Configure** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="693ef-158">Distribuire ed eseguire l'applicazione</span><span class="sxs-lookup"><span data-stu-id="693ef-158">Deploy and Run the Application</span></span>

<span data-ttu-id="693ef-159">Preparare le istanze del Server di Windows per distribuire l'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="693ef-159">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="693ef-160">Aggiungere il ruolo IIS.</span><span class="sxs-lookup"><span data-stu-id="693ef-160">Add the IIS role.</span></span> <span data-ttu-id="693ef-161">Includere le funzionalità di "Sviluppo di applicazioni", inclusi il protocollo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="693ef-161">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="693ef-162">Includere anche il servizio di gestione (elencati in "Strumenti di gestione").</span><span class="sxs-lookup"><span data-stu-id="693ef-162">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="693ef-163">**Installare Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="693ef-163">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="693ef-164">Quando si esegue Gestione IIS, verrà chiesto di installare una piattaforma Web Microsoft oppure è possibile [scaricare il intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="693ef-164">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="693ef-165">Nel programma di installazione di piattaforma, eseguire la ricerca di distribuzione Web e installare distribuzione Web 3.0</span><span class="sxs-lookup"><span data-stu-id="693ef-165">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="693ef-166">Verificare che il servizio gestione Web è in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="693ef-166">Check that the Web Management Service is running.</span></span> <span data-ttu-id="693ef-167">In caso contrario, avviare il servizio.</span><span class="sxs-lookup"><span data-stu-id="693ef-167">If not, start the service.</span></span> <span data-ttu-id="693ef-168">(Se il servizio di gestione Web non viene visualizzato nell'elenco dei servizi di Windows, assicurarsi che il servizio di gestione è installato quando si aggiunge il ruolo IIS.)</span><span class="sxs-lookup"><span data-stu-id="693ef-168">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="693ef-169">Infine, aprire la porta 8172 per TCP.</span><span class="sxs-lookup"><span data-stu-id="693ef-169">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="693ef-170">Questa è la porta che utilizza lo strumento di distribuzione Web.</span><span class="sxs-lookup"><span data-stu-id="693ef-170">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="693ef-171">A questo punto si è pronti per distribuire il progetto di Visual Studio dal computer di sviluppo al server.</span><span class="sxs-lookup"><span data-stu-id="693ef-171">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="693ef-172">In Esplora soluzioni fare doppio clic la soluzione e fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="693ef-172">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="693ef-173">Per informazioni dettagliate documentazione sulla distribuzione web, vedere [mappa del contenuto di distribuzione Web per Visual Studio e ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="693ef-173">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="693ef-174">Se si distribuisce l'applicazione a due server, è possibile aprire ogni istanza in una finestra distinta del browser e vedere che possano ricevere messaggi SignalR da altro.</span><span class="sxs-lookup"><span data-stu-id="693ef-174">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="693ef-175">(Naturalmente, in un ambiente di produzione, i due server sarebbe sit dietro il bilanciamento del carico.)</span><span class="sxs-lookup"><span data-stu-id="693ef-175">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="693ef-176">Dopo aver eseguito l'applicazione, è possibile vedere che SignalR ha creato automaticamente tabelle nel database:</span><span class="sxs-lookup"><span data-stu-id="693ef-176">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="693ef-177">SignalR gestisce le tabelle.</span><span class="sxs-lookup"><span data-stu-id="693ef-177">SignalR manages the tables.</span></span> <span data-ttu-id="693ef-178">Fino a quando l'applicazione viene distribuita, non eliminare le righe, modificare la tabella e così via.</span><span class="sxs-lookup"><span data-stu-id="693ef-178">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
