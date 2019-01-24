---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: Scalabilità orizzontale di SignalR con il Bus di servizio di Azure | Microsoft Docs
author: bradygaster
description: Le versioni del software utilizzato in questa versione di Visual Studio 2013 .NET 4.5 SignalR argomento 2 nelle versioni precedenti di questa versione 1.x di SignalR per l'argomento di questo argomento,...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 9f6188ff5f716c20d759f73975d6a8ad522834d8
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836818"
---
<a name="signalr-scaleout-with-azure-service-bus"></a><span data-ttu-id="f814f-103">Scalabilità orizzontale di SignalR con il Bus di servizio di Azure</span><span class="sxs-lookup"><span data-stu-id="f814f-103">SignalR Scaleout with Azure Service Bus</span></span>
====================
<span data-ttu-id="f814f-104">dal [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="f814f-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="f814f-105">In questa esercitazione si distribuirà un'applicazione di SignalR per un ruolo Web di Azure di Windows, utilizzando il backplane del Bus di servizio per distribuire i messaggi per ogni istanza del ruolo.</span><span class="sxs-lookup"><span data-stu-id="f814f-105">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span> <span data-ttu-id="f814f-106">(È anche possibile usare il backplane del Bus di servizio con [App web nel servizio App di Azure](https://docs.microsoft.com/azure/app-service-web/).)</span><span class="sxs-lookup"><span data-stu-id="f814f-106">(You can also use the Service Bus backplane with [web apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="f814f-107">Prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="f814f-107">Prerequisites:</span></span>

- <span data-ttu-id="f814f-108">Un account di Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="f814f-108">A Windows Azure account.</span></span>
- <span data-ttu-id="f814f-109">Il [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="f814f-109">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="f814f-110">Visual Studio 2012 or 2013.</span><span class="sxs-lookup"><span data-stu-id="f814f-110">Visual Studio 2012 or 2013.</span></span>

<span data-ttu-id="f814f-111">È anche compatibile con il backplane del bus di servizio [Service Bus per Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), versione 1.1.</span><span class="sxs-lookup"><span data-stu-id="f814f-111">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="f814f-112">Tuttavia, non compatibile con la versione 1.0 di Service Bus per Windows Server.</span><span class="sxs-lookup"><span data-stu-id="f814f-112">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="f814f-113">Pricing</span><span class="sxs-lookup"><span data-stu-id="f814f-113">Pricing</span></span>

<span data-ttu-id="f814f-114">Backplane del Bus di servizio Usa gli argomenti per inviare messaggi.</span><span class="sxs-lookup"><span data-stu-id="f814f-114">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="f814f-115">Per informazioni più aggiornate sui prezzi, vedere [del Bus di servizio](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="f814f-115">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="f814f-116">Al momento della stesura di questo articolo, è possibile inviare 1.000.000 di messaggi al mese per meno di $1.</span><span class="sxs-lookup"><span data-stu-id="f814f-116">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="f814f-117">Backplane invia un messaggio del bus di servizio per ogni chiamata di un metodo dell'hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="f814f-117">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="f814f-118">Esistono anche alcuni messaggi di controllo per le connessioni, disconnessioni, unita tramite join o uscire da gruppi e così via.</span><span class="sxs-lookup"><span data-stu-id="f814f-118">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="f814f-119">Nella maggior parte delle applicazioni, la maggior parte del traffico di messaggi sarà chiamate del metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="f814f-119">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="f814f-120">Panoramica</span><span class="sxs-lookup"><span data-stu-id="f814f-120">Overview</span></span>

<span data-ttu-id="f814f-121">Prima di passare all'esercitazione dettagliata, ecco una rapida panoramica delle azioni da eseguire.</span><span class="sxs-lookup"><span data-stu-id="f814f-121">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="f814f-122">Usare il portale di Azure per creare un nuovo spazio dei nomi del Bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="f814f-122">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="f814f-123">Aggiungere i pacchetti NuGet per l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="f814f-123">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="f814f-124">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="f814f-124">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - <span data-ttu-id="f814f-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) or [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span><span class="sxs-lookup"><span data-stu-id="f814f-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) or [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span></span>
3. <span data-ttu-id="f814f-126">Creare un'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="f814f-126">Create a SignalR application.</span></span>
4. <span data-ttu-id="f814f-127">Aggiungere il codice seguente per configurare backplane Startup.cs:</span><span class="sxs-lookup"><span data-stu-id="f814f-127">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="f814f-128">Questo codice consente di configurare con i valori predefiniti per il backplane [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) e [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="f814f-128">This code configures the backplane with the default values for [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="f814f-129">Per informazioni sulla modifica di questi valori, vedere [prestazioni di SignalR: La metrica di scalabilità orizzontale](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="f814f-129">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

<span data-ttu-id="f814f-130">Per ogni applicazione, selezionare un valore diverso per "YourAppName".</span><span class="sxs-lookup"><span data-stu-id="f814f-130">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="f814f-131">Non utilizzare lo stesso valore tra più applicazioni.</span><span class="sxs-lookup"><span data-stu-id="f814f-131">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="f814f-132">Creare i servizi di Azure</span><span class="sxs-lookup"><span data-stu-id="f814f-132">Create the Azure Services</span></span>

<span data-ttu-id="f814f-133">Creare un servizio Cloud, come descritto in [come creare e distribuire un servizio Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="f814f-133">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="f814f-134">Seguire i passaggi nella sezione "procedura: Creare un servizio cloud con creazione rapida".</span><span class="sxs-lookup"><span data-stu-id="f814f-134">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="f814f-135">Per questa esercitazione, non occorre caricare un certificato.</span><span class="sxs-lookup"><span data-stu-id="f814f-135">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="f814f-136">Creare un nuovo spazio dei nomi del Bus di servizio, come descritto in [procedura per usare Service Bus argomenti/sottoscrizioni](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="f814f-136">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="f814f-137">Seguire i passaggi nella sezione "Creare un servizio Namespace".</span><span class="sxs-lookup"><span data-stu-id="f814f-137">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="f814f-138">Assicurarsi di selezionare la stessa area per il servizio cloud e lo spazio dei nomi del Bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="f814f-138">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="f814f-139">Creare il progetto di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f814f-139">Create the Visual Studio Project</span></span>

<span data-ttu-id="f814f-140">Avviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f814f-140">Start Visual Studio.</span></span> <span data-ttu-id="f814f-141">Dal **File** menu, fare clic su **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="f814f-141">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="f814f-142">Nel **nuovo progetto** finestra di dialogo espandere **Visual c#**.</span><span class="sxs-lookup"><span data-stu-id="f814f-142">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="f814f-143">Sotto **modelli installati**, selezionare **Cloud** e quindi selezionare **servizio Cloud Azure**.</span><span class="sxs-lookup"><span data-stu-id="f814f-143">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="f814f-144">Mantenere il valore predefinito di .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="f814f-144">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="f814f-145">Denominare l'applicazione ChatService e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="f814f-145">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="f814f-146">Nel **nuovo servizio Cloud Azure** finestra di dialogo, selezionare un ruolo Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f814f-146">In the **New Windows Azure Cloud Service** dialog, select ASP.NET Web Role.</span></span> <span data-ttu-id="f814f-147">Fare clic sul pulsante freccia destra (**&gt;**) per aggiungere il ruolo alla soluzione.</span><span class="sxs-lookup"><span data-stu-id="f814f-147">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="f814f-148">Posizionare il mouse sul nuovo ruolo, quindi sull'icona della matita visibile.</span><span class="sxs-lookup"><span data-stu-id="f814f-148">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="f814f-149">Fare clic su questa icona per rinominare il ruolo.</span><span class="sxs-lookup"><span data-stu-id="f814f-149">Click this icon to rename the role.</span></span> <span data-ttu-id="f814f-150">Nome del ruolo "SignalRChat", quindi scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="f814f-150">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="f814f-151">Nel **nuovo progetto ASP.NET** finestra di dialogo, seleziona **MVC**, fare clic su OK.</span><span class="sxs-lookup"><span data-stu-id="f814f-151">In the **New ASP.NET Project** dialog, select **MVC**, and click OK.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="f814f-152">La creazione guidata progetto crea due progetti:</span><span class="sxs-lookup"><span data-stu-id="f814f-152">The project wizard creates two projects:</span></span>

- <span data-ttu-id="f814f-153">ChatService: Questo progetto è l'applicazione di Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="f814f-153">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="f814f-154">Definisce i ruoli di Azure e altre opzioni di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f814f-154">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="f814f-155">SignalRChat: Questo progetto è il progetto ASP.NET MVC 5.</span><span class="sxs-lookup"><span data-stu-id="f814f-155">SignalRChat: This project is your ASP.NET MVC 5 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="f814f-156">Creare l'applicazione di Chat di SignalR</span><span class="sxs-lookup"><span data-stu-id="f814f-156">Create the SignalR Chat Application</span></span>

<span data-ttu-id="f814f-157">Per creare l'applicazione di chat, seguire i passaggi nell'esercitazione [Introduzione a SignalR e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="f814f-157">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="f814f-158">Usare NuGet per installare le librerie necessarie.</span><span class="sxs-lookup"><span data-stu-id="f814f-158">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="f814f-159">Dal **degli strumenti** dal menu **Gestione pacchetti NuGet**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="f814f-159">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="f814f-160">Nel **Console di gestione pacchetti** finestra, immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f814f-160">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="f814f-161">Usare il `-ProjectName` opzione per installare i pacchetti per il progetto ASP.NET MVC, piuttosto che il progetto Azure.</span><span class="sxs-lookup"><span data-stu-id="f814f-161">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="f814f-162">Configurare il Backplane</span><span class="sxs-lookup"><span data-stu-id="f814f-162">Configure the Backplane</span></span>

<span data-ttu-id="f814f-163">Nel file Startup.cs dell'applicazione, aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f814f-163">In your application's Startup.cs file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="f814f-164">A questo punto è necessario ottenere la stringa di connessione del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="f814f-164">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="f814f-165">Nel portale di Azure, selezionare lo spazio dei nomi del bus di servizio è stato creato e fare clic sull'icona di tasto di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="f814f-165">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

<span data-ttu-id="f814f-166">Copiare la stringa di connessione negli Appunti, quindi incollarlo nella *connectionString* variabile.</span><span class="sxs-lookup"><span data-stu-id="f814f-166">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="f814f-167">Distribuire in Azure</span><span class="sxs-lookup"><span data-stu-id="f814f-167">Deploy to Azure</span></span>

<span data-ttu-id="f814f-168">In Esplora soluzioni espandere la **ruoli** cartella all'interno del progetto ChatService.</span><span class="sxs-lookup"><span data-stu-id="f814f-168">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="f814f-169">Il ruolo SignalRChat e scegliere **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="f814f-169">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="f814f-170">Selezionare il **configurazione** scheda. Sotto **istanze** selezionare 2.</span><span class="sxs-lookup"><span data-stu-id="f814f-170">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="f814f-171">È anche possibile impostare le dimensioni VM su **molto piccola**.</span><span class="sxs-lookup"><span data-stu-id="f814f-171">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="f814f-172">Salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="f814f-172">Save the changes.</span></span>

<span data-ttu-id="f814f-173">In Esplora soluzioni fare clic sul progetto le ChatService.</span><span class="sxs-lookup"><span data-stu-id="f814f-173">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="f814f-174">Selezionare **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="f814f-174">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="f814f-175">Se questa è la prima pubblicazione ora Windows Azure, è necessario scaricare le credenziali.</span><span class="sxs-lookup"><span data-stu-id="f814f-175">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="f814f-176">Nel **pubblica** procedura guidata, fare clic su "Accedi scaricare le credenziali".</span><span class="sxs-lookup"><span data-stu-id="f814f-176">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="f814f-177">Verrà chiesto di accedere al portale di Azure e scaricare un file di impostazioni di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="f814f-177">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="f814f-178">Fare clic su **importazione** e selezionare il file di impostazioni di pubblicazione scaricato.</span><span class="sxs-lookup"><span data-stu-id="f814f-178">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="f814f-179">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="f814f-179">Click **Next**.</span></span> <span data-ttu-id="f814f-180">Nel **impostazioni di pubblicazione** finestra di dialogo, sotto **servizio Cloud**, selezionare il servizio cloud creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="f814f-180">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="f814f-181">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="f814f-181">Click **Publish**.</span></span> <span data-ttu-id="f814f-182">Possono volerci alcuni minuti per distribuire l'applicazione e avviare le macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="f814f-182">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="f814f-183">A questo punto quando si esegue l'applicazione di chat, istanze del ruolo di comunicano tramite il Bus di servizio di Azure, usando un argomento del Bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="f814f-183">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="f814f-184">Un argomento è una coda di messaggi che consente a più sottoscrittori.</span><span class="sxs-lookup"><span data-stu-id="f814f-184">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="f814f-185">Backplane crea automaticamente gli argomenti e sottoscrizioni.</span><span class="sxs-lookup"><span data-stu-id="f814f-185">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="f814f-186">Per visualizzare le sottoscrizioni e attività del messaggio, aprire il portale di Azure, selezionare lo spazio dei nomi del Bus di servizio e fare clic su "Argomenti".</span><span class="sxs-lookup"><span data-stu-id="f814f-186">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="f814f-187">Rende necessari alcuni minuti per l'attività del messaggio da visualizzare nel dashboard.</span><span class="sxs-lookup"><span data-stu-id="f814f-187">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

<span data-ttu-id="f814f-188">SignalR gestisce la durata di argomento.</span><span class="sxs-lookup"><span data-stu-id="f814f-188">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="f814f-189">Fino a quando l'applicazione viene distribuita, non tentare di eliminare gli argomenti manualmente o modificare le impostazioni per l'argomento.</span><span class="sxs-lookup"><span data-stu-id="f814f-189">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="f814f-190">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="f814f-190">Troubleshooting</span></span>

<span data-ttu-id="f814f-191">**System. InvalidOperationException "L'unico IsolationLevel supportato è 'IsolationLevel.Serializable'".**</span><span class="sxs-lookup"><span data-stu-id="f814f-191">**System.InvalidOperationException "The only supported IsolationLevel is 'IsolationLevel.Serializable'."**</span></span>

<span data-ttu-id="f814f-192">Questo errore può verificarsi se il livello di transazione per un'operazione è impostato su un valore diverso da `Serializable`.</span><span class="sxs-lookup"><span data-stu-id="f814f-192">This error can occur if the transaction level for an operation is set to something other than `Serializable`.</span></span> <span data-ttu-id="f814f-193">Verificare che nessuna operazione sono viene eseguita con altri livelli delle transazioni.</span><span class="sxs-lookup"><span data-stu-id="f814f-193">Verify that no operations are being performed with other transaction levels.</span></span>
