---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: Scalabilità orizzontale di SignalR con SQL Server (SignalR 1.x) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 05/01/2013
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: cd0e3d4bdb4d2eb78e5c41167a17f8673584f654
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832103"
---
<a name="signalr-scaleout-with-sql-server-signalr-1x"></a><span data-ttu-id="b0eeb-102">Scalabilità orizzontale di SignalR con SQL Server (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="b0eeb-102">SignalR Scaleout with SQL Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="b0eeb-103">dal [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="b0eeb-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="b0eeb-104">In questa esercitazione si userà SQL Server per distribuire i messaggi tra un'applicazione di SignalR distribuito in due istanze separate di IIS.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-104">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="b0eeb-105">È anche possibile eseguire questa esercitazione in un computer singolo test, ma per ottenere l'effetto completo, è necessario distribuire l'applicazione di SignalR in due o più server.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-105">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="b0eeb-106">È anche necessario installare SQL Server in uno dei server o in un server dedicato distinto.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-106">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="b0eeb-107">Un'altra opzione consiste nell'eseguire l'esercitazione usando le macchine virtuali in Azure.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-107">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="b0eeb-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="b0eeb-108">Prerequisites</span></span>

<span data-ttu-id="b0eeb-109">Microsoft SQL Server 2005 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-109">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="b0eeb-110">Backplane supporta le edizioni desktop e server di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-110">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="b0eeb-111">Non supporta SQL Server Compact Edition o Database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-111">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="b0eeb-112">(Se l'applicazione è ospitata in Azure, prendere in considerazione il backplane del Bus di servizio invece.)</span><span class="sxs-lookup"><span data-stu-id="b0eeb-112">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="b0eeb-113">Panoramica</span><span class="sxs-lookup"><span data-stu-id="b0eeb-113">Overview</span></span>

<span data-ttu-id="b0eeb-114">Prima di passare all'esercitazione dettagliata, ecco una rapida panoramica delle azioni da eseguire.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-114">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="b0eeb-115">Creare un nuovo database vuoto.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-115">Create a new empty database.</span></span> <span data-ttu-id="b0eeb-116">Backplane creerà le tabelle necessarie in questo database.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-116">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="b0eeb-117">Aggiungere i pacchetti NuGet per l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="b0eeb-117">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="b0eeb-118">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="b0eeb-118">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="b0eeb-119">Microsoft.AspNet.SignalR.SqlServer</span><span class="sxs-lookup"><span data-stu-id="b0eeb-119">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="b0eeb-120">Creare un'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-120">Create a SignalR application.</span></span>
4. <span data-ttu-id="b0eeb-121">Aggiungere il codice seguente al Global. asax per configurare il backplane:</span><span class="sxs-lookup"><span data-stu-id="b0eeb-121">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a><span data-ttu-id="b0eeb-122">Configurare il Database</span><span class="sxs-lookup"><span data-stu-id="b0eeb-122">Configure the Database</span></span>

<span data-ttu-id="b0eeb-123">Decidere se l'applicazione utilizzerà l'autenticazione di Windows o SQL Server per accedere al database.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-123">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="b0eeb-124">Indipendentemente dal fatto che, assicurarsi che l'utente del database disponga delle autorizzazioni per accedere, creare gli schemi e creare tabelle.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-124">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="b0eeb-125">Creare un nuovo database per il backplane da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-125">Create a new database for the backplane to use.</span></span> <span data-ttu-id="b0eeb-126">È possibile assegnare al database un nome qualsiasi.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-126">You can give the database any name.</span></span> <span data-ttu-id="b0eeb-127">Non è necessario creare tutte le tabelle nel database. backplane verrà create le tabelle necessarie.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-127">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="b0eeb-128">Abilitare Service Broker</span><span class="sxs-lookup"><span data-stu-id="b0eeb-128">Enable Service Broker</span></span>

<span data-ttu-id="b0eeb-129">È consigliabile abilitare Service Broker per il database backplane.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-129">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="b0eeb-130">Service Broker fornisce il supporto nativo per la messaggistica e Accodamento in SQL Server, che consente il backplane di ricevere gli aggiornamenti in modo più efficiente.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-130">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="b0eeb-131">(Tuttavia, il backplane funziona anche senza Service Broker.)</span><span class="sxs-lookup"><span data-stu-id="b0eeb-131">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="b0eeb-132">Per verificare se Service Broker è abilitato, eseguire una query il **viene\_broker\_abilitata** colonna il **Sys. Databases** vista del catalogo.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-132">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="b0eeb-133">Per abilitare Service Broker, usare la query SQL seguente:</span><span class="sxs-lookup"><span data-stu-id="b0eeb-133">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="b0eeb-134">Se questa query viene visualizzato un deadlock, assicurarsi che non sono presenti applicazioni connesse al database.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-134">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>


<span data-ttu-id="b0eeb-135">Se è stata abilitata la traccia, le tracce verranno inoltre descritto se Service Broker è abilitato.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-135">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="b0eeb-136">Creare un'applicazione di SignalR</span><span class="sxs-lookup"><span data-stu-id="b0eeb-136">Create a SignalR Application</span></span>

<span data-ttu-id="b0eeb-137">Creare un'applicazione di SignalR seguendo una di queste esercitazioni:</span><span class="sxs-lookup"><span data-stu-id="b0eeb-137">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="b0eeb-138">Introduzione a SignalR</span><span class="sxs-lookup"><span data-stu-id="b0eeb-138">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="b0eeb-139">Introduzione a SignalR e MVC 4</span><span class="sxs-lookup"><span data-stu-id="b0eeb-139">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="b0eeb-140">Successivamente, modifichiamo l'applicazione di chat per supportare scalabilità orizzontale con SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-140">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="b0eeb-141">In primo luogo, aggiungere il pacchetto SignalR.SqlServer NuGet al progetto.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-141">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="b0eeb-142">In Visual Studio dal **degli strumenti** dal menu **Library Package Manager**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-142">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="b0eeb-143">Nella finestra della Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b0eeb-143">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="b0eeb-144">Successivamente, aprire il file Global. asax.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-144">Next, open the Global.asax file.</span></span> <span data-ttu-id="b0eeb-145">Aggiungere il codice seguente per il **Application\_avviare** metodo:</span><span class="sxs-lookup"><span data-stu-id="b0eeb-145">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="b0eeb-146">Distribuire ed eseguire l'applicazione</span><span class="sxs-lookup"><span data-stu-id="b0eeb-146">Deploy and Run the Application</span></span>

<span data-ttu-id="b0eeb-147">Preparare le istanze di Windows Server per distribuire l'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-147">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="b0eeb-148">Aggiungere il ruolo IIS.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-148">Add the IIS role.</span></span> <span data-ttu-id="b0eeb-149">Include funzionalità di "Sviluppo di applicazioni", tra cui il protocollo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-149">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="b0eeb-150">Includere anche il servizio di gestione (elencati in "Strumenti di gestione").</span><span class="sxs-lookup"><span data-stu-id="b0eeb-150">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="b0eeb-151">**Installare Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="b0eeb-151">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="b0eeb-152">Quando si esegue Gestione IIS, verrà richiesto di installare piattaforma Web Microsoft oppure è possibile [Scarica il intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="b0eeb-152">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="b0eeb-153">Nel programma di installazione della piattaforma, eseguire la ricerca di Web Deploy e installare distribuzione Web 3.0</span><span class="sxs-lookup"><span data-stu-id="b0eeb-153">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="b0eeb-154">Verificare che il servizio di gestione Web sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-154">Check that the Web Management Service is running.</span></span> <span data-ttu-id="b0eeb-155">In caso contrario, avviare il servizio.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-155">If not, start the service.</span></span> <span data-ttu-id="b0eeb-156">(Se il servizio di gestione Web non viene visualizzata nell'elenco dei servizi di Windows, assicurarsi che il servizio di gestione installato quando è stato aggiunto il ruolo IIS.)</span><span class="sxs-lookup"><span data-stu-id="b0eeb-156">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="b0eeb-157">Infine, aprire la porta 8172 per TCP.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-157">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="b0eeb-158">Questa è la porta che utilizza lo strumento distribuzione Web.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-158">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="b0eeb-159">A questo punto si è pronti per distribuire il progetto di Visual Studio dal computer di sviluppo al server.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-159">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="b0eeb-160">In Esplora soluzioni fare doppio clic la soluzione e fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-160">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="b0eeb-161">Per informazioni dettagliate documentazione sulla distribuzione web, vedere [mappa del contenuto di distribuzione Web per Visual Studio e ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="b0eeb-161">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="b0eeb-162">Se si distribuisce l'applicazione a due server, è possibile aprire ogni istanza in una finestra distinta del browser e vedere che possano ricevere i messaggi SignalR da un altro.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-162">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="b0eeb-163">(Naturalmente, in un ambiente di produzione, i due server sarebbe si trovano dietro un bilanciamento del carico.)</span><span class="sxs-lookup"><span data-stu-id="b0eeb-163">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="b0eeb-164">Dopo aver eseguito l'applicazione, è possibile vedere che SignalR ha creato automaticamente le tabelle nel database:</span><span class="sxs-lookup"><span data-stu-id="b0eeb-164">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="b0eeb-165">SignalR gestisce le tabelle.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-165">SignalR manages the tables.</span></span> <span data-ttu-id="b0eeb-166">Fino a quando l'applicazione viene distribuita, non eliminare le righe, modificare la tabella e così via.</span><span class="sxs-lookup"><span data-stu-id="b0eeb-166">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
