---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Ospitare API Web ASP.NET 2 in un ruolo di lavoro di Azure | Microsoft Docs
author: MikeWasson
description: Questa esercitazione illustra come eseguire l'hosting di API Web ASP.NET in un ruolo di lavoro di Azure, Usa OWIN per l'hosting indipendente il framework API Web. Open Web Interface per de .NET (OWIN)...
ms.author: riande
ms.date: 04/02/2014
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: cabf88e4e6c946f92a9e4534a4db5ae15dd8cae5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829509"
---
<a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a><span data-ttu-id="c1d5f-104">Ospitare API Web ASP.NET 2 in un ruolo di lavoro di Azure</span><span class="sxs-lookup"><span data-stu-id="c1d5f-104">Host ASP.NET Web API 2 in an Azure Worker Role</span></span>
====================
<span data-ttu-id="c1d5f-105">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c1d5f-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="c1d5f-106">Questa esercitazione illustra come eseguire l'hosting di API Web ASP.NET in un ruolo di lavoro di Azure, Usa OWIN per l'hosting indipendente il framework API Web.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-106">This tutorial shows how to host ASP.NET Web API in an Azure Worker Role, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="c1d5f-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) definisce un'astrazione tra i server web .NET e applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="c1d5f-108">OWIN consente di disaccoppiare l'applicazione web dal server, che rende ideale per un'applicazione web nel proprio processo, all'esterno di IIS di self-hosting OWIN – ad esempio, all'interno di un ruolo di lavoro di Azure.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
> 
> <span data-ttu-id="c1d5f-109">In questa esercitazione si userà il pacchetto Microsoft.Owin.Host.HttpListener, che fornisce un server HTTP che consente di self-hosting delle applicazioni OWIN.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-109">In this tutorial, you'll use the Microsoft.Owin.Host.HttpListener package, which provides an HTTP server that be used to self-host OWIN applications.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c1d5f-110">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="c1d5f-110">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="c1d5f-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="c1d5f-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="c1d5f-112">API Web 2</span><span class="sxs-lookup"><span data-stu-id="c1d5f-112">Web API 2</span></span>
> - [<span data-ttu-id="c1d5f-113">Azure SDK per .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="c1d5f-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="c1d5f-114">Creare un progetto di Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="c1d5f-114">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="c1d5f-115">Avviare Visual Studio con privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-115">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="c1d5f-116">Sono necessari privilegi di amministratore per eseguire il debug in locale, l'applicazione usa l'emulatore di calcolo di Azure.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-116">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="c1d5f-117">Nel **File** menu, fare clic su **New**, quindi fare clic su **progetto**.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-117">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="c1d5f-118">Dal **modelli installati**, in Visual c#, fare clic su **Cloud** e quindi fare clic su **servizio Cloud Azure**.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-118">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="c1d5f-119">Denominare il progetto "AzureApp" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-119">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="c1d5f-120">Nel **nuovo servizio Cloud di Windows Azure** finestra di dialogo, fare doppio clic su **ruolo di lavoro**.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-120">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="c1d5f-121">Lasciare il nome predefinito ("WorkerRole1").</span><span class="sxs-lookup"><span data-stu-id="c1d5f-121">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="c1d5f-122">Questo passaggio aggiunge un ruolo di lavoro alla soluzione.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-122">This step adds a worker role to the solution.</span></span> <span data-ttu-id="c1d5f-123">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-123">Click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="c1d5f-124">La soluzione di Visual Studio creato contiene due progetti:</span><span class="sxs-lookup"><span data-stu-id="c1d5f-124">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="c1d5f-125">&quot;AzureApp&quot; definisce i ruoli e la configurazione per l'applicazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-125">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="c1d5f-126">&quot;WorkerRole1&quot; contiene il codice per il ruolo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-126">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="c1d5f-127">In generale, un'applicazione Azure può contenere più ruoli, sebbene questa esercitazione Usa un singolo ruolo.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-127">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="c1d5f-128">Aggiungere l'API Web e pacchetti OWIN</span><span class="sxs-lookup"><span data-stu-id="c1d5f-128">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="c1d5f-129">Dal **degli strumenti** menu, fare clic su **Library Package Manager**, quindi fare clic su **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-129">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="c1d5f-130">Nella finestra della Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c1d5f-130">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="c1d5f-131">Aggiungere un HTTP Endpoint</span><span class="sxs-lookup"><span data-stu-id="c1d5f-131">Add an HTTP Endpoint</span></span>

<span data-ttu-id="c1d5f-132">In Esplora soluzioni espandere il progetto AzureApp.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-132">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="c1d5f-133">Espandere il nodo ruoli, selezionare e fare doppio clic su WorkerRole1 **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-133">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="c1d5f-134">Fare clic su **endpoint**, quindi fare clic su **Add Endpoint**.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-134">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="c1d5f-135">Nel **protocollo** dall'elenco a discesa selezionare "http".</span><span class="sxs-lookup"><span data-stu-id="c1d5f-135">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="c1d5f-136">Nelle **porta pubblica** e **porta privata**, digitare 80.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-136">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="c1d5f-137">Questi numeri di porta possono essere diversi.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-137">These port numbers can be different.</span></span> <span data-ttu-id="c1d5f-138">La porta pubblica è ciò che i client usano quando si invia una richiesta al ruolo.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-138">The public port is what clients use when they send a request to the role.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="c1d5f-139">Configurare Web API per l'hosting indipendente</span><span class="sxs-lookup"><span data-stu-id="c1d5f-139">Configure Web API for Self-Host</span></span>

<span data-ttu-id="c1d5f-140">In Esplora soluzioni fare clic con il pulsante destro del progetto WorkerRole1 e selezionare **Add** / **classe** per aggiungere una nuova classe.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-140">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="c1d5f-141">Assegnare alla classe il nome `Startup`.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-141">Name the class `Startup`.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="c1d5f-142">Sostituire tutto il codice boilerplate in questo file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c1d5f-142">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="c1d5f-143">Aggiungere un Controller API Web</span><span class="sxs-lookup"><span data-stu-id="c1d5f-143">Add a Web API Controller</span></span>

<span data-ttu-id="c1d5f-144">Successivamente, aggiungere una classe controller API Web.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-144">Next, add a Web API controller class.</span></span> <span data-ttu-id="c1d5f-145">Fare clic sul progetto WorkerRole1 e selezionare **Add** / **classe**.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-145">Right-click the WorkerRole1 project and select **Add** / **Class**.</span></span> <span data-ttu-id="c1d5f-146">Nome della classe TestController.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-146">Name the class TestController.</span></span> <span data-ttu-id="c1d5f-147">Sostituire tutto il codice boilerplate in questo file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c1d5f-147">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="c1d5f-148">Per semplicità, questo controller definisce solo due metodi GET che restituiscono testo normale.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-148">For simplicity, this controller just defines two GET methods that return plain text.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="c1d5f-149">Avviare l'Host OWIN</span><span class="sxs-lookup"><span data-stu-id="c1d5f-149">Start the OWIN Host</span></span>

<span data-ttu-id="c1d5f-150">Aprire il file WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-150">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="c1d5f-151">Questa classe definisce il codice eseguito quando il ruolo di lavoro viene avviato e arrestato.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-151">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="c1d5f-152">Aggiungere la seguente istruzione using:</span><span class="sxs-lookup"><span data-stu-id="c1d5f-152">Add the following using statement:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="c1d5f-153">Aggiungere un **IDisposable** membro per il `WorkerRole` classe:</span><span class="sxs-lookup"><span data-stu-id="c1d5f-153">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

<span data-ttu-id="c1d5f-154">Nel `OnStart` metodo, aggiungere il codice seguente per avviare l'host:</span><span class="sxs-lookup"><span data-stu-id="c1d5f-154">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

<span data-ttu-id="c1d5f-155">Il **WebApp.Start** metodo avvia l'host OWIN.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-155">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="c1d5f-156">Il nome del `Startup` classe è un parametro di tipo al metodo.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-156">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="c1d5f-157">Per convenzione, l'host chiamerà il `Configure` overload di questa classe.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-157">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="c1d5f-158">Eseguire l'override di `OnStop` per eliminare il  *\_app* istanza:</span><span class="sxs-lookup"><span data-stu-id="c1d5f-158">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="c1d5f-159">Ecco il codice completo per WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="c1d5f-159">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

<span data-ttu-id="c1d5f-160">Compilare la soluzione e premere F5 per eseguire l'applicazione in locale nell'emulatore di calcolo di Azure.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-160">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="c1d5f-161">A seconda delle impostazioni di firewall, potrebbe essere necessario consentire all'emulatore attraverso il firewall.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-161">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="c1d5f-162">Se si verifica un'eccezione simile al seguente, vedi [questo post di blog](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) per una soluzione alternativa.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-162">If you get an exception like the following, please see [this blog post](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) for a workaround.</span></span> <span data-ttu-id="c1d5f-163">"Impossibile caricare il file o l'assembly ' owin, versione = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' o una delle relative dipendenze.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-163">"Could not load file or assembly 'Microsoft.Owin, Version=2.0.2.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.</span></span> <span data-ttu-id="c1d5f-164">Definizione del manifesto dell'assembly individuato non corrisponde il riferimento all'assembly.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-164">The located assembly's manifest definition does not match the assembly reference.</span></span> <span data-ttu-id="c1d5f-165">(Eccezione da HRESULT: 0x80131040) "</span><span class="sxs-lookup"><span data-stu-id="c1d5f-165">(Exception from HRESULT: 0x80131040)"</span></span>


<span data-ttu-id="c1d5f-166">L'emulatore di calcolo viene assegnato un indirizzo IP locale per l'endpoint.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-166">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="c1d5f-167">È possibile trovare l'indirizzo IP, visualizzare l'interfaccia utente emulatore di calcolo.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-167">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="c1d5f-168">Fare doppio clic sull'icona dell'emulatore nella barra area di notifica delle attività e selezionare **Mostra interfaccia emulatore di calcolo**.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-168">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

<span data-ttu-id="c1d5f-169">Trovare l'indirizzo IP sotto le distribuzioni del servizio, la distribuzione [id], i dettagli del servizio.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-169">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="c1d5f-170">Aprire un web browser e passare a http://<em>indirizzi</em>/test/1, dove <em>indirizzo</em> è l'indirizzo IP assegnato dall'emulatore di calcolo; ad esempio, `http://127.0.0.1:80/test/1`.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-170">Open a web browser and navigate to http://<em>address</em>/test/1, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80/test/1`.</span></span> <span data-ttu-id="c1d5f-171">Verrà visualizzata la risposta dal controller API Web:</span><span class="sxs-lookup"><span data-stu-id="c1d5f-171">You should see the response from the Web API controller:</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="c1d5f-172">Distribuire in Azure</span><span class="sxs-lookup"><span data-stu-id="c1d5f-172">Deploy to Azure</span></span>

<span data-ttu-id="c1d5f-173">Per questo passaggio, è necessario disporre di un account Azure.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-173">For this step, you must have an Azure account.</span></span> <span data-ttu-id="c1d5f-174">Se non si ha già uno, è possibile creare un account di valutazione gratuito in pochi minuti.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-174">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="c1d5f-175">Per informazioni dettagliate, vedere [versione di valutazione gratuita di Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="c1d5f-175">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="c1d5f-176">In Esplora soluzioni fare clic sul progetto AzureApp.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-176">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="c1d5f-177">Selezionare **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-177">Select **Publish**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="c1d5f-178">Se non è stato eseguito l'accesso al proprio account Azure, fare clic su **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-178">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

<span data-ttu-id="c1d5f-179">Dopo che è stato effettuato, scegliere una sottoscrizione e fare clic su **successivo**.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-179">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

<span data-ttu-id="c1d5f-180">Immettere un nome per il servizio cloud e scegliere un'area.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-180">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="c1d5f-181">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-181">Click **Create**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="c1d5f-182">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-182">Click **Publish**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

<span data-ttu-id="c1d5f-183">La finestra Log attività di Azure Mostra lo stato di avanzamento della distribuzione.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-183">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="c1d5f-184">Quando l'app viene distribuita, passa a http://appname.cloudapp.net/test/1.</span><span class="sxs-lookup"><span data-stu-id="c1d5f-184">When the app is deployed, browse to http://appname.cloudapp.net/test/1.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a><span data-ttu-id="c1d5f-185">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c1d5f-185">Additional Resources</span></span>

- [<span data-ttu-id="c1d5f-186">Panoramica del progetto Katana</span><span class="sxs-lookup"><span data-stu-id="c1d5f-186">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [<span data-ttu-id="c1d5f-187">Katana Project su GitHub</span><span class="sxs-lookup"><span data-stu-id="c1d5f-187">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana)
