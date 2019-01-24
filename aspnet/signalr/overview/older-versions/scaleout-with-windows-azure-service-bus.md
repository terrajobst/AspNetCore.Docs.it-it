---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Scalabilità orizzontale di SignalR con il Bus di servizio di Azure (SignalR 1.x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 77186f43b38a8423a1cbd4cf42723c5b9ccdd953
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837910"
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a><span data-ttu-id="af86c-102">Scalabilità orizzontale di SignalR con il Bus di servizio di Azure (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="af86c-102">SignalR Scaleout with Azure Service Bus (SignalR 1.x)</span></span>
====================
<span data-ttu-id="af86c-103">dal [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="af86c-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="af86c-104">In questa esercitazione si distribuirà un'applicazione di SignalR per un ruolo Web di Azure di Windows, utilizzando il backplane del Bus di servizio per distribuire i messaggi per ogni istanza del ruolo.</span><span class="sxs-lookup"><span data-stu-id="af86c-104">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="af86c-105">Prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="af86c-105">Prerequisites:</span></span>

- <span data-ttu-id="af86c-106">Un account di Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="af86c-106">A Windows Azure account.</span></span>
- <span data-ttu-id="af86c-107">Il [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="af86c-107">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="af86c-108">Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="af86c-108">Visual Studio 2012.</span></span>

<span data-ttu-id="af86c-109">È anche compatibile con il backplane del bus di servizio [Service Bus per Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), versione 1.1.</span><span class="sxs-lookup"><span data-stu-id="af86c-109">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="af86c-110">Tuttavia, non compatibile con la versione 1.0 di Service Bus per Windows Server.</span><span class="sxs-lookup"><span data-stu-id="af86c-110">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="af86c-111">Pricing</span><span class="sxs-lookup"><span data-stu-id="af86c-111">Pricing</span></span>

<span data-ttu-id="af86c-112">Backplane del Bus di servizio Usa gli argomenti per inviare messaggi.</span><span class="sxs-lookup"><span data-stu-id="af86c-112">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="af86c-113">Per informazioni più aggiornate sui prezzi, vedere [del Bus di servizio](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="af86c-113">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="af86c-114">Al momento della stesura di questo articolo, è possibile inviare 1.000.000 di messaggi al mese per meno di $1.</span><span class="sxs-lookup"><span data-stu-id="af86c-114">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="af86c-115">Backplane invia un messaggio del bus di servizio per ogni chiamata di un metodo dell'hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="af86c-115">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="af86c-116">Esistono anche alcuni messaggi di controllo per le connessioni, disconnessioni, unita tramite join o uscire da gruppi e così via.</span><span class="sxs-lookup"><span data-stu-id="af86c-116">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="af86c-117">Nella maggior parte delle applicazioni, la maggior parte del traffico di messaggi sarà chiamate del metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="af86c-117">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="af86c-118">Panoramica</span><span class="sxs-lookup"><span data-stu-id="af86c-118">Overview</span></span>

<span data-ttu-id="af86c-119">Prima di passare all'esercitazione dettagliata, ecco una rapida panoramica delle azioni da eseguire.</span><span class="sxs-lookup"><span data-stu-id="af86c-119">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="af86c-120">Usare il portale di Azure per creare un nuovo spazio dei nomi del Bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="af86c-120">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="af86c-121">Aggiungere i pacchetti NuGet per l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="af86c-121">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="af86c-122">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="af86c-122">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="af86c-123">Microsoft.AspNet.SignalR.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="af86c-123">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="af86c-124">Creare un'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="af86c-124">Create a SignalR application.</span></span>
4. <span data-ttu-id="af86c-125">Aggiungere il codice seguente al Global. asax per configurare il backplane:</span><span class="sxs-lookup"><span data-stu-id="af86c-125">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="af86c-126">Per ogni applicazione, selezionare un valore diverso per "YourAppName".</span><span class="sxs-lookup"><span data-stu-id="af86c-126">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="af86c-127">Non utilizzare lo stesso valore tra più applicazioni.</span><span class="sxs-lookup"><span data-stu-id="af86c-127">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="af86c-128">Creare i servizi di Azure</span><span class="sxs-lookup"><span data-stu-id="af86c-128">Create the Azure Services</span></span>

<span data-ttu-id="af86c-129">Creare un servizio Cloud, come descritto in [come creare e distribuire un servizio Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="af86c-129">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="af86c-130">Seguire i passaggi nella sezione "procedura: Creare un servizio cloud con creazione rapida".</span><span class="sxs-lookup"><span data-stu-id="af86c-130">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="af86c-131">Per questa esercitazione, non occorre caricare un certificato.</span><span class="sxs-lookup"><span data-stu-id="af86c-131">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="af86c-132">Creare un nuovo spazio dei nomi del Bus di servizio, come descritto in [procedura per usare Service Bus argomenti/sottoscrizioni](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="af86c-132">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="af86c-133">Seguire i passaggi nella sezione "Creare un servizio Namespace".</span><span class="sxs-lookup"><span data-stu-id="af86c-133">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="af86c-134">Assicurarsi di selezionare la stessa area per il servizio cloud e lo spazio dei nomi del Bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="af86c-134">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="af86c-135">Creare il progetto di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="af86c-135">Create the Visual Studio Project</span></span>

<span data-ttu-id="af86c-136">Avviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="af86c-136">Start Visual Studio.</span></span> <span data-ttu-id="af86c-137">Dal **File** menu, fare clic su **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="af86c-137">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="af86c-138">Nel **nuovo progetto** finestra di dialogo espandere **Visual c#**.</span><span class="sxs-lookup"><span data-stu-id="af86c-138">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="af86c-139">Sotto **modelli installati**, selezionare **Cloud** e quindi selezionare **servizio Cloud Azure**.</span><span class="sxs-lookup"><span data-stu-id="af86c-139">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="af86c-140">Mantenere il valore predefinito di .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="af86c-140">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="af86c-141">Denominare l'applicazione ChatService e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="af86c-141">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="af86c-142">Nel **nuovo servizio Cloud Azure** finestra di dialogo, selezionare Web ruoli di ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="af86c-142">In the **New Windows Azure Cloud Service** dialog, select ASP.NET MVC 4 Web Role.</span></span> <span data-ttu-id="af86c-143">Fare clic sul pulsante freccia destra (**&gt;**) per aggiungere il ruolo alla soluzione.</span><span class="sxs-lookup"><span data-stu-id="af86c-143">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="af86c-144">Posizionare il mouse sul nuovo ruolo, quindi sull'icona della matita visibile.</span><span class="sxs-lookup"><span data-stu-id="af86c-144">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="af86c-145">Fare clic su questa icona per rinominare il ruolo.</span><span class="sxs-lookup"><span data-stu-id="af86c-145">Click this icon to rename the role.</span></span> <span data-ttu-id="af86c-146">Nome del ruolo "SignalRChat", quindi scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="af86c-146">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="af86c-147">Nel **nuovo progetto ASP.NET MVC 4** procedura guidata, selezionare **applicazione Internet**.</span><span class="sxs-lookup"><span data-stu-id="af86c-147">In the **New ASP.NET MVC 4 Project** wizard, select **Internet Application**.</span></span> <span data-ttu-id="af86c-148">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="af86c-148">Click **OK**.</span></span> <span data-ttu-id="af86c-149">La creazione guidata progetto crea due progetti:</span><span class="sxs-lookup"><span data-stu-id="af86c-149">The project wizard creates two projects:</span></span>

- <span data-ttu-id="af86c-150">ChatService: Questo progetto è l'applicazione di Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="af86c-150">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="af86c-151">Definisce i ruoli di Azure e altre opzioni di configurazione.</span><span class="sxs-lookup"><span data-stu-id="af86c-151">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="af86c-152">SignalRChat: Questo progetto è il progetto ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="af86c-152">SignalRChat: This project is your ASP.NET MVC 4 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="af86c-153">Creare l'applicazione di Chat di SignalR</span><span class="sxs-lookup"><span data-stu-id="af86c-153">Create the SignalR Chat Application</span></span>

<span data-ttu-id="af86c-154">Per creare l'applicazione di chat, seguire i passaggi nell'esercitazione [Introduzione a SignalR e MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="af86c-154">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="af86c-155">Usare NuGet per installare le librerie necessarie.</span><span class="sxs-lookup"><span data-stu-id="af86c-155">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="af86c-156">Dal **degli strumenti** dal menu **Gestione pacchetti NuGet**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="af86c-156">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="af86c-157">Nel **Console di gestione pacchetti** finestra, immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="af86c-157">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="af86c-158">Usare il `-ProjectName` opzione per installare i pacchetti per il progetto ASP.NET MVC, piuttosto che il progetto Azure.</span><span class="sxs-lookup"><span data-stu-id="af86c-158">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="af86c-159">Configurare il Backplane</span><span class="sxs-lookup"><span data-stu-id="af86c-159">Configure the Backplane</span></span>

<span data-ttu-id="af86c-160">Nel file Global. asax dell'applicazione, aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="af86c-160">In your application's Global.asax file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="af86c-161">A questo punto è necessario ottenere la stringa di connessione del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="af86c-161">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="af86c-162">Nel portale di Azure, selezionare lo spazio dei nomi del bus di servizio è stato creato e fare clic sull'icona di tasto di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="af86c-162">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="af86c-163">Copiare la stringa di connessione negli Appunti, quindi incollarlo nella *connectionString* variabile.</span><span class="sxs-lookup"><span data-stu-id="af86c-163">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="af86c-164">Distribuire in Azure</span><span class="sxs-lookup"><span data-stu-id="af86c-164">Deploy to Azure</span></span>

<span data-ttu-id="af86c-165">In Esplora soluzioni espandere la **ruoli** cartella all'interno del progetto ChatService.</span><span class="sxs-lookup"><span data-stu-id="af86c-165">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

<span data-ttu-id="af86c-166">Il ruolo SignalRChat e scegliere **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="af86c-166">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="af86c-167">Selezionare il **configurazione** scheda. Sotto **istanze** selezionare 2.</span><span class="sxs-lookup"><span data-stu-id="af86c-167">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="af86c-168">È anche possibile impostare le dimensioni VM su **molto piccola**.</span><span class="sxs-lookup"><span data-stu-id="af86c-168">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="af86c-169">Salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="af86c-169">Save the changes.</span></span>

<span data-ttu-id="af86c-170">In Esplora soluzioni fare clic sul progetto le ChatService.</span><span class="sxs-lookup"><span data-stu-id="af86c-170">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="af86c-171">Selezionare **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="af86c-171">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="af86c-172">Se questa è la prima pubblicazione ora Windows Azure, è necessario scaricare le credenziali.</span><span class="sxs-lookup"><span data-stu-id="af86c-172">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="af86c-173">Nel **pubblica** procedura guidata, fare clic su "Accedi scaricare le credenziali".</span><span class="sxs-lookup"><span data-stu-id="af86c-173">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="af86c-174">Verrà chiesto di accedere al portale di Azure e scaricare un file di impostazioni di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="af86c-174">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="af86c-175">Fare clic su **importazione** e selezionare il file di impostazioni di pubblicazione scaricato.</span><span class="sxs-lookup"><span data-stu-id="af86c-175">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="af86c-176">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="af86c-176">Click **Next**.</span></span> <span data-ttu-id="af86c-177">Nel **impostazioni di pubblicazione** finestra di dialogo, sotto **servizio Cloud**, selezionare il servizio cloud creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="af86c-177">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="af86c-178">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="af86c-178">Click **Publish**.</span></span> <span data-ttu-id="af86c-179">Possono volerci alcuni minuti per distribuire l'applicazione e avviare le macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="af86c-179">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="af86c-180">A questo punto quando si esegue l'applicazione di chat, istanze del ruolo di comunicano tramite il Bus di servizio di Azure, usando un argomento del Bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="af86c-180">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="af86c-181">Un argomento è una coda di messaggi che consente a più sottoscrittori.</span><span class="sxs-lookup"><span data-stu-id="af86c-181">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="af86c-182">Backplane crea automaticamente gli argomenti e sottoscrizioni.</span><span class="sxs-lookup"><span data-stu-id="af86c-182">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="af86c-183">Per visualizzare le sottoscrizioni e attività del messaggio, aprire il portale di Azure, selezionare lo spazio dei nomi del Bus di servizio e fare clic su "Argomenti".</span><span class="sxs-lookup"><span data-stu-id="af86c-183">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="af86c-184">Rende necessari alcuni minuti per l'attività del messaggio da visualizzare nel dashboard.</span><span class="sxs-lookup"><span data-stu-id="af86c-184">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="af86c-185">SignalR gestisce la durata di argomento.</span><span class="sxs-lookup"><span data-stu-id="af86c-185">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="af86c-186">Fino a quando l'applicazione viene distribuita, non tentare di eliminare gli argomenti manualmente o modificare le impostazioni per l'argomento.</span><span class="sxs-lookup"><span data-stu-id="af86c-186">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>
