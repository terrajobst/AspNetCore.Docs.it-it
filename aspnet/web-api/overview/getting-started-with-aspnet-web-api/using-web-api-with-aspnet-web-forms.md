---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Using Web API with ASP.NET Web Forms | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: af918671a8bcc97a0050ea033ccd14dd96416e73
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/01/2017
---
<a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="80990-102">Using Web API with ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="80990-102">Using Web API with ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="80990-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="80990-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="80990-104">Anche se viene creato un pacchetto di ASP.NET Web API with ASP.NET MVC, è facile aggiungere API Web a un'applicazione Web Form ASP.NET tradizionale.</span><span class="sxs-lookup"><span data-stu-id="80990-104">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span> <span data-ttu-id="80990-105">In questa esercitazione vengono illustrati i passaggi.</span><span class="sxs-lookup"><span data-stu-id="80990-105">This tutorial walks you through the steps.</span></span>

## <a name="overview"></a><span data-ttu-id="80990-106">Panoramica</span><span class="sxs-lookup"><span data-stu-id="80990-106">Overview</span></span>

<span data-ttu-id="80990-107">Per utilizzare l'API Web in un'applicazione Web Form, sono disponibili due passaggi principali:</span><span class="sxs-lookup"><span data-stu-id="80990-107">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="80990-108">Aggiungere un controller API Web da cui deriva il **ApiController** classe.</span><span class="sxs-lookup"><span data-stu-id="80990-108">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="80990-109">Aggiungere una tabella di route per il **applicazione\_avviare** metodo.</span><span class="sxs-lookup"><span data-stu-id="80990-109">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="80990-110">Creare un progetto di Web Form</span><span class="sxs-lookup"><span data-stu-id="80990-110">Create a Web Forms Project</span></span>

<span data-ttu-id="80990-111">Avviare Visual Studio e selezionare **nuovo progetto** dal **avviare** pagina.</span><span class="sxs-lookup"><span data-stu-id="80990-111">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="80990-112">O dal **File** dal menu **New** e quindi **progetto**.</span><span class="sxs-lookup"><span data-stu-id="80990-112">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="80990-113">Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il **Visual c#** nodo.</span><span class="sxs-lookup"><span data-stu-id="80990-113">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="80990-114">In **Visual c#**selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="80990-114">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="80990-115">Nell'elenco dei modelli di progetto, selezionare **applicazioni Web Form ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="80990-115">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="80990-116">Immettere un nome per il progetto e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="80990-116">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="80990-117">Creare il modello e il Controller</span><span class="sxs-lookup"><span data-stu-id="80990-117">Create the Model and Controller</span></span>

<span data-ttu-id="80990-118">In questa esercitazione Usa le classi di modello e il controller stesso come il [Introduzione](tutorial-your-first-web-api.md) esercitazione.</span><span class="sxs-lookup"><span data-stu-id="80990-118">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="80990-119">Aggiungere innanzitutto una classe di modello.</span><span class="sxs-lookup"><span data-stu-id="80990-119">First, add a model class.</span></span> <span data-ttu-id="80990-120">In **Esplora**, fare clic sul progetto e selezionare **Aggiungi classe**.</span><span class="sxs-lookup"><span data-stu-id="80990-120">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="80990-121">Nome della classe di prodotto, quindi aggiungere l'implementazione del seguente:</span><span class="sxs-lookup"><span data-stu-id="80990-121">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="80990-122">Successivamente, aggiungere un controller API Web al progetto., A *controller* è l'oggetto che gestisce le richieste HTTP per l'API Web.</span><span class="sxs-lookup"><span data-stu-id="80990-122">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="80990-123">In **Esplora**, fare clic sul progetto.</span><span class="sxs-lookup"><span data-stu-id="80990-123">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="80990-124">Selezionare **Aggiungi nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="80990-124">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="80990-125">In **modelli installati**, espandere **Visual c#** e selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="80990-125">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="80990-126">Quindi, nell'elenco dei modelli, selezionare **classe Controller di Web API**.</span><span class="sxs-lookup"><span data-stu-id="80990-126">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="80990-127">Denominare il controller "ProductsController" e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="80990-127">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="80990-128">Il **Aggiungi nuovo elemento** procedura guidata creerà un file denominato ProductsController.cs.</span><span class="sxs-lookup"><span data-stu-id="80990-128">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="80990-129">Eliminare i metodi inclusi nella procedura guidata e aggiungere i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="80990-129">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="80990-130">Per ulteriori informazioni sul codice in questo controller, vedere il [Introduzione](tutorial-your-first-web-api.md) esercitazione.</span><span class="sxs-lookup"><span data-stu-id="80990-130">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="80990-131">Aggiungere le informazioni di Routing</span><span class="sxs-lookup"><span data-stu-id="80990-131">Add Routing Information</span></span>

<span data-ttu-id="80990-132">Successivamente, aggiungeremo una route URI in modo che gli URI del form &quot;/api/prodotti/&quot; vengono indirizzati al controller.</span><span class="sxs-lookup"><span data-stu-id="80990-132">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="80990-133">In **Esplora**, fare doppio clic su Global. asax per aprire il file code-behind Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="80990-133">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="80990-134">Aggiungere il seguente **utilizzando** istruzione.</span><span class="sxs-lookup"><span data-stu-id="80990-134">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="80990-135">Quindi aggiungere il codice seguente per il **applicazione\_avviare** metodo:</span><span class="sxs-lookup"><span data-stu-id="80990-135">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="80990-136">Per ulteriori informazioni sulle tabelle di routine, vedere [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="80990-136">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="80990-137">Aggiungere AJAX lato Client</span><span class="sxs-lookup"><span data-stu-id="80990-137">Add Client-Side AJAX</span></span>

<span data-ttu-id="80990-138">Questo è tutto che è necessario creare una web API che i client possono accedere.</span><span class="sxs-lookup"><span data-stu-id="80990-138">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="80990-139">Ora aggiungere una pagina HTML che utilizza jQuery per chiamare l'API.</span><span class="sxs-lookup"><span data-stu-id="80990-139">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="80990-140">Verificare che la pagina master (ad esempio, *Site. master*) include un `ContentPlaceHolder` con `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="80990-140">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="80990-141">Aprire il file Default.aspx.</span><span class="sxs-lookup"><span data-stu-id="80990-141">Open the file Default.aspx.</span></span> <span data-ttu-id="80990-142">Sostituire il testo boilerplate nella sezione di contenuto principale, come illustrato:</span><span class="sxs-lookup"><span data-stu-id="80990-142">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="80990-143">Successivamente, aggiungere un riferimento al file di origine jQuery nel `HeaderContent` sezione:</span><span class="sxs-lookup"><span data-stu-id="80990-143">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="80990-144">Nota: È possibile facilmente aggiungere il riferimento allo script trascinando e rilasciando il file da **Esplora** nella finestra dell'editor di codice.</span><span class="sxs-lookup"><span data-stu-id="80990-144">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="80990-145">Sotto il tag di script jQuery, aggiungere il blocco di script seguenti:</span><span class="sxs-lookup"><span data-stu-id="80990-145">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="80990-146">Quando si carica il documento, lo script effettua una richiesta di AJAX per &quot;api/prodotti&quot;.</span><span class="sxs-lookup"><span data-stu-id="80990-146">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="80990-147">La richiesta restituisce un elenco di prodotti in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="80990-147">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="80990-148">Lo script aggiunge le informazioni sul prodotto per la tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="80990-148">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="80990-149">Quando si esegue l'applicazione, dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="80990-149">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
