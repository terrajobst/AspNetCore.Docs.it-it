---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Scalabilità orizzontale SignalR con il Bus di servizio di Azure (SignalR 1. x) | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2013
ms.topic: article
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: b48a7b04701b69f68a492c0f7e08da4a37a92a48
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2018
ms.locfileid: "28036440"
---
<a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a><span data-ttu-id="f8cd4-102">Scalabilità orizzontale SignalR con il Bus di servizio di Azure (SignalR 1. x)</span><span class="sxs-lookup"><span data-stu-id="f8cd4-102">SignalR Scaleout with Azure Service Bus (SignalR 1.x)</span></span>
====================
<span data-ttu-id="f8cd4-103">dal [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="f8cd4-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="f8cd4-104">In questa esercitazione, si distribuirà un'applicazione di SignalR per un ruolo Web di Azure di Windows, con il backplane del Bus di servizio per distribuire i messaggi per ogni istanza del ruolo.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-104">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="f8cd4-105">Prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="f8cd4-105">Prerequisites:</span></span>

- <span data-ttu-id="f8cd4-106">Un account di Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-106">A Windows Azure account.</span></span>
- <span data-ttu-id="f8cd4-107">Il [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="f8cd4-107">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="f8cd4-108">Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-108">Visual Studio 2012.</span></span>

<span data-ttu-id="f8cd4-109">Backplane di bus di servizio è inoltre compatibile con [Service Bus per Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), versione 1.1.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-109">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="f8cd4-110">Non è tuttavia compatibile con la versione 1.0 di Service Bus per Windows Server.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-110">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="f8cd4-111">Pricing</span><span class="sxs-lookup"><span data-stu-id="f8cd4-111">Pricing</span></span>

<span data-ttu-id="f8cd4-112">Bus di servizio backplane utilizza argomenti per inviare messaggi.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-112">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="f8cd4-113">Per informazioni più recenti sui prezzi, vedere [Bus di servizio](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="f8cd4-113">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="f8cd4-114">Al momento della redazione del presente documento, è possibile inviare 1.000.000 messaggi al mese per meno di $1.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-114">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="f8cd4-115">Backplane invia un messaggio del bus di servizio per ogni chiamata di un metodo dell'hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-115">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="f8cd4-116">Esistono inoltre alcuni messaggi di controllo per le connessioni, disconnessioni, unione o uscire da gruppi e così via.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-116">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="f8cd4-117">Nella maggior parte delle applicazioni, la maggior parte del traffico dei messaggi sarà chiamate del metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-117">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="f8cd4-118">Panoramica</span><span class="sxs-lookup"><span data-stu-id="f8cd4-118">Overview</span></span>

<span data-ttu-id="f8cd4-119">Prima di passare all'esercitazione dettagliata, ecco una rapida panoramica delle azioni da eseguire.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-119">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="f8cd4-120">Utilizzare il portale Windows Azure per creare un nuovo spazio dei nomi Service Bus.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-120">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="f8cd4-121">Aggiungere questi pacchetti NuGet per l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="f8cd4-121">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="f8cd4-122">Microsoft.AspNet.SignalR</span><span class="sxs-lookup"><span data-stu-id="f8cd4-122">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="f8cd4-123">Microsoft.AspNet.SignalR.ServiceBus</span><span class="sxs-lookup"><span data-stu-id="f8cd4-123">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="f8cd4-124">Creare un'applicazione di SignalR.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-124">Create a SignalR application.</span></span>
4. <span data-ttu-id="f8cd4-125">Aggiungere il codice seguente per Global. asax per configurare il backplane:</span><span class="sxs-lookup"><span data-stu-id="f8cd4-125">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="f8cd4-126">Per ogni applicazione, selezionare un valore diverso per "NomeApp".</span><span class="sxs-lookup"><span data-stu-id="f8cd4-126">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="f8cd4-127">Non utilizzare lo stesso valore in più applicazioni.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-127">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="f8cd4-128">Creare i servizi di Azure</span><span class="sxs-lookup"><span data-stu-id="f8cd4-128">Create the Azure Services</span></span>

<span data-ttu-id="f8cd4-129">Creare un servizio Cloud, come descritto in [come creare e distribuire un servizio Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="f8cd4-129">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="f8cd4-130">Seguire i passaggi nella sezione "procedura: creare un servizio cloud utilizzando Creazione rapida".</span><span class="sxs-lookup"><span data-stu-id="f8cd4-130">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="f8cd4-131">Per questa esercitazione, non è necessario caricare un certificato.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-131">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="f8cd4-132">Creare un nuovo spazio dei nomi Service Bus, come descritto in [come al Bus di servizio utilizzare argomenti/sottoscrizioni](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="f8cd4-132">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="f8cd4-133">Seguire i passaggi nella sezione "Creare un servizio Namespace".</span><span class="sxs-lookup"><span data-stu-id="f8cd4-133">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="f8cd4-134">Assicurarsi di selezionare la stessa area per il servizio cloud e lo spazio dei nomi del Bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-134">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="f8cd4-135">Creare il progetto di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8cd4-135">Create the Visual Studio Project</span></span>

<span data-ttu-id="f8cd4-136">Avviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-136">Start Visual Studio.</span></span> <span data-ttu-id="f8cd4-137">Dal **File** menu, fare clic su **nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-137">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="f8cd4-138">Nel **nuovo progetto** finestra di dialogo espandere **Visual c#**.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-138">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="f8cd4-139">In **modelli installati**selezionare **Cloud** e quindi selezionare **servizio Cloud Azure**.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-139">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="f8cd4-140">Mantenere il valore predefinito di .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-140">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="f8cd4-141">L'applicazione ChatService e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-141">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="f8cd4-142">Nel **nuovo servizio Cloud Azure** finestra di dialogo, selezionare Web ruoli di ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-142">In the **New Windows Azure Cloud Service** dialog, select ASP.NET MVC 4 Web Role.</span></span> <span data-ttu-id="f8cd4-143">Fare clic sul pulsante freccia destra (**&gt;**) per aggiungere il ruolo alla soluzione.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-143">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="f8cd4-144">Posizionare il mouse sul nuovo ruolo, quindi sull'icona della matita visibile.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-144">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="f8cd4-145">Fare clic su questa icona per rinominare il ruolo.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-145">Click this icon to rename the role.</span></span> <span data-ttu-id="f8cd4-146">Il ruolo "SignalRChat" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-146">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="f8cd4-147">Nel **nuovo progetto ASP.NET MVC 4** procedura guidata, selezionare **applicazione Internet**.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-147">In the **New ASP.NET MVC 4 Project** wizard, select **Internet Application**.</span></span> <span data-ttu-id="f8cd4-148">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-148">Click **OK**.</span></span> <span data-ttu-id="f8cd4-149">La creazione guidata progetto crea due progetti:</span><span class="sxs-lookup"><span data-stu-id="f8cd4-149">The project wizard creates two projects:</span></span>

- <span data-ttu-id="f8cd4-150">ChatService: Questo progetto è l'applicazione di Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-150">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="f8cd4-151">Definisce i ruoli Azure e altre opzioni di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-151">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="f8cd4-152">SignalRChat: Questo progetto è il progetto ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-152">SignalRChat: This project is your ASP.NET MVC 4 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="f8cd4-153">Creare l'applicazione di Chat di SignalR</span><span class="sxs-lookup"><span data-stu-id="f8cd4-153">Create the SignalR Chat Application</span></span>

<span data-ttu-id="f8cd4-154">Per creare l'applicazione di chat, seguire i passaggi nell'esercitazione [Guida introduttiva a SignalR e MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="f8cd4-154">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="f8cd4-155">Utilizzare NuGet per installare le librerie necessarie.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-155">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="f8cd4-156">Dal **strumenti** dal menu **Gestione pacchetti libreria**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-156">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="f8cd4-157">Nel **Package Manager Console** finestra, immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f8cd4-157">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="f8cd4-158">Utilizzare il `-ProjectName` opzione per installare i pacchetti del progetto ASP.NET MVC, piuttosto che il progetto di Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-158">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="f8cd4-159">Configurare il Backplane</span><span class="sxs-lookup"><span data-stu-id="f8cd4-159">Configure the Backplane</span></span>

<span data-ttu-id="f8cd4-160">Nel file Global. asax dell'applicazione, aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f8cd4-160">In your application's Global.asax file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="f8cd4-161">È necessario ottenere una stringa di connessione del bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-161">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="f8cd4-162">Nel portale di Azure, selezionare i nomi del bus di servizio creato e fare clic sull'icona di tasto di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-162">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="f8cd4-163">Copiare la stringa di connessione negli Appunti, quindi incollarlo nella *connectionString* variabile.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-163">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="f8cd4-164">Distribuire in Azure</span><span class="sxs-lookup"><span data-stu-id="f8cd4-164">Deploy to Azure</span></span>

<span data-ttu-id="f8cd4-165">In Esplora soluzioni, espandere il **ruoli** cartella all'interno del progetto ChatService.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-165">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

<span data-ttu-id="f8cd4-166">Il ruolo SignalRChat di mouse e scegliere **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-166">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="f8cd4-167">Selezionare il **configurazione** scheda. In **istanze** selezionare 2.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-167">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="f8cd4-168">È inoltre possibile impostare le dimensioni della VM su **molto piccolo**.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-168">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="f8cd4-169">Salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-169">Save the changes.</span></span>

<span data-ttu-id="f8cd4-170">In Esplora soluzioni, fare clic sul progetto ChatService.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-170">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="f8cd4-171">Selezionare **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-171">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="f8cd4-172">Se questa è la prima ora la pubblicazione in Windows Azure, è necessario scaricare le credenziali.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-172">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="f8cd4-173">Nel **pubblica** procedura guidata, fare clic su "Accedi scaricare le credenziali".</span><span class="sxs-lookup"><span data-stu-id="f8cd4-173">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="f8cd4-174">Verrà chiesto di accedere al portale di Windows Azure e scaricare un file di impostazioni di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-174">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="f8cd4-175">Fare clic su **importazione** e selezionare il file di impostazioni di pubblicazione che è stato scaricato.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-175">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="f8cd4-176">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-176">Click **Next**.</span></span> <span data-ttu-id="f8cd4-177">Nel **impostazioni di pubblicazione** finestra di dialogo, in **servizio Cloud**, selezionare il servizio cloud creato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-177">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="f8cd4-178">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-178">Click **Publish**.</span></span> <span data-ttu-id="f8cd4-179">Può richiedere alcuni minuti per distribuire l'applicazione e avviare le macchine virtuali.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-179">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="f8cd4-180">A questo punto quando si esegue l'applicazione di chat, le istanze del ruolo comunicano tramite Bus di servizio di Azure, mediante un argomento del Bus di servizio.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-180">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="f8cd4-181">Un argomento è una coda di messaggi che consente a più sottoscrittori.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-181">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="f8cd4-182">Backplane crea automaticamente gli argomenti e le sottoscrizioni.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-182">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="f8cd4-183">Per visualizzare le sottoscrizioni e attività del messaggio, aprire il portale di Azure, selezionare lo spazio dei nomi del Bus di servizio e fare clic su "Argomenti".</span><span class="sxs-lookup"><span data-stu-id="f8cd4-183">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="f8cd4-184">Può richiedere alcuni minuti per l'attività di messaggio da visualizzare nel dashboard.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-184">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="f8cd4-185">SignalR gestisce la durata di argomento.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-185">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="f8cd4-186">Fino a quando l'applicazione viene distribuita, non tentare di eliminare gli argomenti manualmente o modificare le impostazioni per l'argomento.</span><span class="sxs-lookup"><span data-stu-id="f8cd4-186">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>
