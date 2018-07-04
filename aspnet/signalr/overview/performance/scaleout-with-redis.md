---
uid: signalr/overview/performance/scaleout-with-redis
title: Scalabilità orizzontale di SignalR con Redis | Microsoft Docs
author: MikeWasson
description: Le versioni del software utilizzato in questo argomento di Visual Studio 2013 .NET 4.5 SignalR le versioni precedenti la versione 2 di questo argomento per informazioni sulle versioni precedenti di...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: e08e0fdcd0df65bea6802838a8ed55d6b86ce1a3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381955"
---
<a name="signalr-scaleout-with-redis"></a><span data-ttu-id="a7d01-103">Scalabilità orizzontale di SignalR con Redis</span><span class="sxs-lookup"><span data-stu-id="a7d01-103">SignalR Scaleout with Redis</span></span>
====================
<span data-ttu-id="a7d01-104">dal [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="a7d01-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="a7d01-105">Versioni del software utilizzate in questo argomento</span><span class="sxs-lookup"><span data-stu-id="a7d01-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="a7d01-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="a7d01-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="a7d01-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="a7d01-107">.NET 4.5</span></span>
> - <span data-ttu-id="a7d01-108">SignalR versione 2</span><span class="sxs-lookup"><span data-stu-id="a7d01-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="a7d01-109">Versioni precedenti di questo argomento</span><span class="sxs-lookup"><span data-stu-id="a7d01-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="a7d01-110">Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="a7d01-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="a7d01-111">Domande e commenti</span><span class="sxs-lookup"><span data-stu-id="a7d01-111">Questions and comments</span></span>
> 
> <span data-ttu-id="a7d01-112">Inviaci un feedback sul modo in cui è stato apprezzato questa esercitazione e cosa possiamo migliorare nei commenti nella parte inferiore della pagina.</span><span class="sxs-lookup"><span data-stu-id="a7d01-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="a7d01-113">Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oppure [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="a7d01-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="a7d01-114">In questa esercitazione si userà [Redis](http://redis.io/) distribuire i messaggi in un'applicazione di SignalR che viene distribuita in due istanze separate di IIS.</span><span class="sxs-lookup"><span data-stu-id="a7d01-114">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="a7d01-115">Redis è un archivio chiave-valore in memoria.</span><span class="sxs-lookup"><span data-stu-id="a7d01-115">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="a7d01-116">Supporta inoltre un sistema di messaggistica con un modello publish/subscribe.</span><span class="sxs-lookup"><span data-stu-id="a7d01-116">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="a7d01-117">Backplane SignalR Redis Usa la funzionalità di pubblicazione/sottoscrizione per inoltrare i messaggi ad altri server.</span><span class="sxs-lookup"><span data-stu-id="a7d01-117">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="a7d01-118">Per questa esercitazione si userà tre server:</span><span class="sxs-lookup"><span data-stu-id="a7d01-118">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="a7d01-119">Due server che eseguono Windows, che verrà usato per distribuire un'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="a7d01-119">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="a7d01-120">Un server che esegue Linux, che verrà usato per l'esecuzione di Redis.</span><span class="sxs-lookup"><span data-stu-id="a7d01-120">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="a7d01-121">Per le schermate contenute in questa esercitazione, ho utilizzato Ubuntu 12.04 TLS.</span><span class="sxs-lookup"><span data-stu-id="a7d01-121">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="a7d01-122">Se non si dispone di tre server fisici da usare, è possibile creare macchine virtuali in Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="a7d01-122">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="a7d01-123">Un'altra opzione consiste nel creare le macchine virtuali in Azure.</span><span class="sxs-lookup"><span data-stu-id="a7d01-123">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="a7d01-124">Anche se in questa esercitazione Usa l'implementazione di Redis ufficiale, è inoltre disponibile un' [porte Windows di Redis](https://github.com/MSOpenTech/redis) da MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="a7d01-124">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="a7d01-125">Il programma di installazione e configurazione sono diversi, ma in caso contrario, i passaggi sono uguali.</span><span class="sxs-lookup"><span data-stu-id="a7d01-125">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a7d01-126">Scalabilità orizzontale di SignalR con Redis non supporta i cluster Redis.</span><span class="sxs-lookup"><span data-stu-id="a7d01-126">SignalR scaleout with Redis does not support Redis clusters.</span></span>


## <a name="overview"></a><span data-ttu-id="a7d01-127">Panoramica</span><span class="sxs-lookup"><span data-stu-id="a7d01-127">Overview</span></span>

<span data-ttu-id="a7d01-128">Prima di passare all'esercitazione dettagliata, ecco una rapida panoramica delle azioni da eseguire.</span><span class="sxs-lookup"><span data-stu-id="a7d01-128">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="a7d01-129">Installare Redis e avviare il server Redis.</span><span class="sxs-lookup"><span data-stu-id="a7d01-129">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="a7d01-130">Aggiungere i pacchetti NuGet per l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="a7d01-130">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="a7d01-131">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="a7d01-131">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="a7d01-132">Microsoft.AspNet.SignalR.Redis</span><span class="sxs-lookup"><span data-stu-id="a7d01-132">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="a7d01-133">Creare un'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="a7d01-133">Create a SignalR application.</span></span>
4. <span data-ttu-id="a7d01-134">Aggiungere il codice seguente per configurare backplane Startup.cs:</span><span class="sxs-lookup"><span data-stu-id="a7d01-134">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="a7d01-135">Ubuntu in Hyper-V</span><span class="sxs-lookup"><span data-stu-id="a7d01-135">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="a7d01-136">Con Windows Hyper-V, è possibile creare facilmente una VM Ubuntu in Windows Server.</span><span class="sxs-lookup"><span data-stu-id="a7d01-136">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="a7d01-137">Scaricare il file ISO di Ubuntu dalla [ http://www.ubuntu.com ](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="a7d01-137">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="a7d01-138">In Hyper-V, aggiungere una nuova macchina virtuale.</span><span class="sxs-lookup"><span data-stu-id="a7d01-138">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="a7d01-139">Nel **connessione disco rigido virtuale** passaggio, seleziona **creare un disco rigido virtuale**.</span><span class="sxs-lookup"><span data-stu-id="a7d01-139">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="a7d01-140">Nel **opzioni di installazione** passaggio, seleziona **file di immagine (con estensione ISO)**, fare clic su **Sfoglia**e individuare l'ISO di installazione di Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="a7d01-140">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="a7d01-141">Installare Redis</span><span class="sxs-lookup"><span data-stu-id="a7d01-141">Install Redis</span></span>

<span data-ttu-id="a7d01-142">Seguire i passaggi descritti in [ http://redis.io/download ](http://redis.io/download) per scaricare e compilare Redis.</span><span class="sxs-lookup"><span data-stu-id="a7d01-142">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="a7d01-143">Verranno compilati i file binari di Redis `src` directory.</span><span class="sxs-lookup"><span data-stu-id="a7d01-143">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="a7d01-144">Per impostazione predefinita, Redis non richiede una password.</span><span class="sxs-lookup"><span data-stu-id="a7d01-144">By default, Redis does not require a password.</span></span> <span data-ttu-id="a7d01-145">Per impostare una password, modificare il `redis.conf` file che si trova nella directory radice del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="a7d01-145">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="a7d01-146">(Eseguire una copia di backup del file prima di modificarlo!) Aggiungere la seguente direttiva a `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="a7d01-146">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="a7d01-147">Ora avviare il server Redis:</span><span class="sxs-lookup"><span data-stu-id="a7d01-147">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="a7d01-148">Aprire la porta 6379, ovvero la porta predefinita che Redis è in ascolto su.</span><span class="sxs-lookup"><span data-stu-id="a7d01-148">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="a7d01-149">(È possibile modificare il numero di porta nel file di configurazione).</span><span class="sxs-lookup"><span data-stu-id="a7d01-149">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="a7d01-150">Creare l'applicazione di SignalR</span><span class="sxs-lookup"><span data-stu-id="a7d01-150">Create the SignalR Application</span></span>

<span data-ttu-id="a7d01-151">Creare un'applicazione di SignalR seguendo una di queste esercitazioni:</span><span class="sxs-lookup"><span data-stu-id="a7d01-151">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="a7d01-152">Introduzione a SignalR 2.0</span><span class="sxs-lookup"><span data-stu-id="a7d01-152">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="a7d01-153">Introduzione a SignalR 2.0 e MVC 5</span><span class="sxs-lookup"><span data-stu-id="a7d01-153">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="a7d01-154">Successivamente, modifichiamo l'applicazione di chat per supportare scalabilità orizzontale con Redis.</span><span class="sxs-lookup"><span data-stu-id="a7d01-154">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="a7d01-155">In primo luogo, aggiungere il pacchetto SignalR.Redis NuGet al progetto.</span><span class="sxs-lookup"><span data-stu-id="a7d01-155">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="a7d01-156">In Visual Studio dal **degli strumenti** dal menu **Library Package Manager**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="a7d01-156">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="a7d01-157">Nella finestra della Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="a7d01-157">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="a7d01-158">Successivamente, aprire il file Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="a7d01-158">Next, open the Startup.cs file.</span></span> <span data-ttu-id="a7d01-159">Aggiungere il codice seguente per il **configurazione** metodo:</span><span class="sxs-lookup"><span data-stu-id="a7d01-159">Add the following code to the **Configuration** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="a7d01-160">"server" è il nome del server Redis in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="a7d01-160">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="a7d01-161">*porta* è il numero di porta</span><span class="sxs-lookup"><span data-stu-id="a7d01-161">*port* is the port number</span></span>
- <span data-ttu-id="a7d01-162">"password" è la password che è definito nel file conf.</span><span class="sxs-lookup"><span data-stu-id="a7d01-162">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="a7d01-163">"AppName" è qualsiasi stringa.</span><span class="sxs-lookup"><span data-stu-id="a7d01-163">"AppName" is any string.</span></span> <span data-ttu-id="a7d01-164">SignalR crea un canale di Redis pub/sub con questo nome.</span><span class="sxs-lookup"><span data-stu-id="a7d01-164">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="a7d01-165">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a7d01-165">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="a7d01-166">Distribuire ed eseguire l'applicazione</span><span class="sxs-lookup"><span data-stu-id="a7d01-166">Deploy and Run the Application</span></span>

<span data-ttu-id="a7d01-167">Preparare le istanze di Windows Server per distribuire l'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="a7d01-167">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="a7d01-168">Aggiungere il ruolo IIS.</span><span class="sxs-lookup"><span data-stu-id="a7d01-168">Add the IIS role.</span></span> <span data-ttu-id="a7d01-169">Include funzionalità di "Sviluppo di applicazioni", tra cui il protocollo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="a7d01-169">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="a7d01-170">Includere anche il servizio di gestione (elencati in "Strumenti di gestione").</span><span class="sxs-lookup"><span data-stu-id="a7d01-170">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="a7d01-171">**Installare Web Deploy 3.0.**</span><span class="sxs-lookup"><span data-stu-id="a7d01-171">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="a7d01-172">Quando si esegue Gestione IIS, verrà richiesto di installare piattaforma Web Microsoft oppure è possibile [Scarica il intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="a7d01-172">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the intstaller](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="a7d01-173">Nel programma di installazione della piattaforma, eseguire la ricerca di Web Deploy e installare distribuzione Web 3.0</span><span class="sxs-lookup"><span data-stu-id="a7d01-173">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="a7d01-174">Verificare che il servizio di gestione Web sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="a7d01-174">Check that the Web Management Service is running.</span></span> <span data-ttu-id="a7d01-175">In caso contrario, avviare il servizio.</span><span class="sxs-lookup"><span data-stu-id="a7d01-175">If not, start the service.</span></span> <span data-ttu-id="a7d01-176">(Se il servizio di gestione Web non viene visualizzata nell'elenco dei servizi di Windows, assicurarsi che il servizio di gestione installato quando è stato aggiunto il ruolo IIS.)</span><span class="sxs-lookup"><span data-stu-id="a7d01-176">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="a7d01-177">Per impostazione predefinita, il servizio di gestione Web è in ascolto sulla porta TCP 8172.</span><span class="sxs-lookup"><span data-stu-id="a7d01-177">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="a7d01-178">In Windows Firewall, creare una nuova regola in ingresso per consentire il traffico TCP sulla porta 8172.</span><span class="sxs-lookup"><span data-stu-id="a7d01-178">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="a7d01-179">Per altre informazioni, vedere [configurare le regole del Firewall](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="a7d01-179">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="a7d01-180">(Se si ospita le macchine virtuali in Azure, è possibile farlo direttamente nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="a7d01-180">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="a7d01-181">Visualizzare [come configurare gli endpoint a una macchina virtuale](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="a7d01-181">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="a7d01-182">A questo punto si è pronti per distribuire il progetto di Visual Studio dal computer di sviluppo al server.</span><span class="sxs-lookup"><span data-stu-id="a7d01-182">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="a7d01-183">In Esplora soluzioni fare doppio clic la soluzione e fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="a7d01-183">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="a7d01-184">Per informazioni dettagliate documentazione sulla distribuzione web, vedere [mappa del contenuto di distribuzione Web per Visual Studio e ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="a7d01-184">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="a7d01-185">Se si distribuisce l'applicazione a due server, è possibile aprire ogni istanza in una finestra distinta del browser e vedere che possano ricevere i messaggi SignalR da un altro.</span><span class="sxs-lookup"><span data-stu-id="a7d01-185">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="a7d01-186">(Naturalmente, in un ambiente di produzione, i due server sarebbe si trovano dietro un bilanciamento del carico.)</span><span class="sxs-lookup"><span data-stu-id="a7d01-186">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="a7d01-187">Se si è interessati a visualizzare i messaggi che vengono inviate a Redis, è possibile usare la **redis-cli** client, che viene installato con Redis.</span><span class="sxs-lookup"><span data-stu-id="a7d01-187">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
