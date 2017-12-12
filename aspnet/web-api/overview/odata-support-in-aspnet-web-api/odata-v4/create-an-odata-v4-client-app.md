---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Creare un'App Client di OData v4 (c#) | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: daa39fbbb4ff17d61f71bf2a642a9c2260b353e4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="create-an-odata-v4-client-app-c"></a><span data-ttu-id="bff58-102">Creare un'App Client di OData v4 (c#)</span><span class="sxs-lookup"><span data-stu-id="bff58-102">Create an OData v4 Client App (C#)</span></span>
====================
<span data-ttu-id="bff58-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bff58-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="bff58-104">Nell'esercitazione precedente, creato un servizio OData di base che supporta operazioni CRUD.</span><span class="sxs-lookup"><span data-stu-id="bff58-104">In the previous tutorial, you created a basic OData service that supports CRUD operations.</span></span> <span data-ttu-id="bff58-105">Ora è possibile crearne un client per il servizio.</span><span class="sxs-lookup"><span data-stu-id="bff58-105">Now let's create a client for the service.</span></span>

<span data-ttu-id="bff58-106">Avviare una nuova istanza di Visual Studio e creare un nuovo progetto applicazione console.</span><span class="sxs-lookup"><span data-stu-id="bff58-106">Start a new instance of Visual Studio and create a new console application project.</span></span> <span data-ttu-id="bff58-107">Nel **nuovo progetto** finestra di dialogo Seleziona **installato** &gt; **modelli** &gt; **Visual c#** &gt; **Windows Desktop**e selezionare il **applicazione Console** modello.</span><span class="sxs-lookup"><span data-stu-id="bff58-107">In the **New Project** dialog, select **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Windows Desktop**, and select the **Console Application** template.</span></span> <span data-ttu-id="bff58-108">Denominare il progetto &quot;ProductsApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="bff58-108">Name the project &quot;ProductsApp&quot;.</span></span>

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="bff58-109">È anche possibile aggiungere l'applicazione console alla stessa soluzione di Visual Studio che contiene il servizio OData.</span><span class="sxs-lookup"><span data-stu-id="bff58-109">You can also add the console app to the same Visual Studio solution that contains the OData service.</span></span>


## <a name="install-the-odata-client-code-generator"></a><span data-ttu-id="bff58-110">Installare il generatore di codice Client OData</span><span class="sxs-lookup"><span data-stu-id="bff58-110">Install the OData Client Code Generator</span></span>

<span data-ttu-id="bff58-111">Dal **strumenti** dal menu **estensioni e aggiornamenti**.</span><span class="sxs-lookup"><span data-stu-id="bff58-111">From the **Tools** menu, select **Extensions and Updates**.</span></span> <span data-ttu-id="bff58-112">Selezionare **Online** &gt; **Visual Studio Gallery**.</span><span class="sxs-lookup"><span data-stu-id="bff58-112">Select **Online** &gt; **Visual Studio Gallery**.</span></span> <span data-ttu-id="bff58-113">Nella casella di ricerca, cercare &quot;generatore di codice Client OData&quot;.</span><span class="sxs-lookup"><span data-stu-id="bff58-113">In the search box, search for &quot;OData Client Code Generator&quot;.</span></span> <span data-ttu-id="bff58-114">Fare clic su **scaricare** per installare l'estensione VSIX.</span><span class="sxs-lookup"><span data-stu-id="bff58-114">Click **Download** to install the VSIX.</span></span> <span data-ttu-id="bff58-115">Potrebbe essere necessario riavviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bff58-115">You might be prompted to restart Visual Studio.</span></span>

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a><span data-ttu-id="bff58-116">Esegue il servizio OData in locale</span><span class="sxs-lookup"><span data-stu-id="bff58-116">Run the OData Service Locally</span></span>

<span data-ttu-id="bff58-117">Eseguire il progetto ProductService da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bff58-117">Run the ProductService project from Visual Studio.</span></span> <span data-ttu-id="bff58-118">Per impostazione predefinita, Visual Studio avvia un browser per la radice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="bff58-118">By default, Visual Studio launches a browser to the application root.</span></span> <span data-ttu-id="bff58-119">Prendere nota dell'URI; è necessario nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="bff58-119">Note the URI; you will need this in the next step.</span></span> <span data-ttu-id="bff58-120">Lasciare l'applicazione in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="bff58-120">Leave the application running.</span></span>

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="bff58-121">Se si inseriscono entrambi i progetti nella stessa soluzione, assicurarsi di eseguire il progetto ProductService senza debug.</span><span class="sxs-lookup"><span data-stu-id="bff58-121">If you put both projects in the same solution, make sure to run the ProductService project without debugging.</span></span> <span data-ttu-id="bff58-122">Nel passaggio successivo, sarà necessario mantenere il servizio in esecuzione mentre si modifica il progetto di applicazione console.</span><span class="sxs-lookup"><span data-stu-id="bff58-122">In the next step, you will need to keep the service running while you modify the console application project.</span></span>


## <a name="generate-the-service-proxy"></a><span data-ttu-id="bff58-123">Generare il Proxy del servizio</span><span class="sxs-lookup"><span data-stu-id="bff58-123">Generate the Service Proxy</span></span>

<span data-ttu-id="bff58-124">Il proxy del servizio è una classe .NET che definisce i metodi per l'accesso al servizio OData.</span><span class="sxs-lookup"><span data-stu-id="bff58-124">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="bff58-125">Il proxy converte le chiamate di metodo in richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="bff58-125">The proxy translates method calls into HTTP requests.</span></span> <span data-ttu-id="bff58-126">Si creerà la classe proxy eseguendo un [modello T4](https://msdn.microsoft.com/en-us/library/bb126445.aspx).</span><span class="sxs-lookup"><span data-stu-id="bff58-126">You will create the proxy class by running a [T4 template](https://msdn.microsoft.com/en-us/library/bb126445.aspx).</span></span>

<span data-ttu-id="bff58-127">Fare clic sul progetto.</span><span class="sxs-lookup"><span data-stu-id="bff58-127">Right-click the project.</span></span> <span data-ttu-id="bff58-128">Selezionare **aggiungere** &gt; **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="bff58-128">Select **Add** &gt; **New Item**.</span></span>

![](create-an-odata-v4-client-app/_static/image5.png)

<span data-ttu-id="bff58-129">Nel **Aggiungi nuovo elemento** finestra di dialogo Seleziona **elementi di Visual c#** &gt; **codice** &gt; **OData Client**.</span><span class="sxs-lookup"><span data-stu-id="bff58-129">In the **Add New Item** dialog, select **Visual C# Items** &gt; **Code** &gt; **OData Client**.</span></span> <span data-ttu-id="bff58-130">Nome del modello &quot;ProductClient.tt&quot;.</span><span class="sxs-lookup"><span data-stu-id="bff58-130">Name the template &quot;ProductClient.tt&quot;.</span></span> <span data-ttu-id="bff58-131">Fare clic su **Aggiungi** e fare clic su tramite l'avviso di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="bff58-131">Click **Add** and click through the security warning.</span></span>

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

<span data-ttu-id="bff58-132">A questo punto, si otterrà un errore, che è possibile ignorare.</span><span class="sxs-lookup"><span data-stu-id="bff58-132">At this point, you'll get an error, which you can ignore.</span></span> <span data-ttu-id="bff58-133">Visual Studio esegue automaticamente il modello, ma il modello debba essere alcune impostazioni di configurazione prima.</span><span class="sxs-lookup"><span data-stu-id="bff58-133">Visual Studio automatically runs the template, but the template needs some configuration settings first.</span></span>

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

<span data-ttu-id="bff58-134">Aprire il file ProductClient.odata.config. Nel `Parameter` elemento Incolla dell'URI dal progetto ProductService (passaggio precedente).</span><span class="sxs-lookup"><span data-stu-id="bff58-134">Open the file ProductClient.odata.config. In the `Parameter` element, paste in the URI from the ProductService project (previous step).</span></span> <span data-ttu-id="bff58-135">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="bff58-135">For example:</span></span>

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

<span data-ttu-id="bff58-136">Eseguire nuovamente il modello.</span><span class="sxs-lookup"><span data-stu-id="bff58-136">Run the template again.</span></span> <span data-ttu-id="bff58-137">In Esplora soluzioni, fare clic con il pulsante destro del file ProductClient.tt e selezionare **Esegui strumento personalizzato**.</span><span class="sxs-lookup"><span data-stu-id="bff58-137">In Solution Explorer, right click the ProductClient.tt file and select **Run Custom Tool**.</span></span>

<span data-ttu-id="bff58-138">Il modello crea un file di codice denominato ProductClient.cs che definisce il proxy.</span><span class="sxs-lookup"><span data-stu-id="bff58-138">The template creates a code file named ProductClient.cs that defines the proxy.</span></span> <span data-ttu-id="bff58-139">Quando si sviluppa l'app, se si modifica l'endpoint OData, eseguire il modello di nuovo per aggiornare il proxy.</span><span class="sxs-lookup"><span data-stu-id="bff58-139">As you develop your app, if you change the OData endpoint, run the template again to update the proxy.</span></span>

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a><span data-ttu-id="bff58-140">Utilizzare il Proxy del servizio per chiamare il servizio OData</span><span class="sxs-lookup"><span data-stu-id="bff58-140">Use the Service Proxy to Call the OData Service</span></span>

<span data-ttu-id="bff58-141">Aprire il file Program.cs e sostituire il codice boilerplate con le operazioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="bff58-141">Open the file Program.cs and replace the boilerplate code with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

<span data-ttu-id="bff58-142">Sostituire il valore di *serviceUri* con l'URI del servizio da versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="bff58-142">Replace the value of *serviceUri* with the service URI from earlier.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

<span data-ttu-id="bff58-143">Quando si esegue l'app, deve restituire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="bff58-143">When you run the app, it should output the following:</span></span>

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
