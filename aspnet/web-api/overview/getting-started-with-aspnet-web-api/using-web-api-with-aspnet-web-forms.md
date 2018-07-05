---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Uso dell'API Web con Web Form ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: 91811031b937d3ca05888ce8d4f28bcc33537c76
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400879"
---
<a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="64f05-102">Uso dell'API Web con Web Form ASP.NET</span><span class="sxs-lookup"><span data-stu-id="64f05-102">Using Web API with ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="64f05-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="64f05-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="64f05-104">Anche se viene incluso nel pacchetto di API Web ASP.NET con ASP.NET MVC, è facile aggiungere l'API Web a un'applicazione Web Form ASP.NET tradizionale.</span><span class="sxs-lookup"><span data-stu-id="64f05-104">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span> <span data-ttu-id="64f05-105">Questa esercitazione illustra i passaggi.</span><span class="sxs-lookup"><span data-stu-id="64f05-105">This tutorial walks you through the steps.</span></span>

## <a name="overview"></a><span data-ttu-id="64f05-106">Panoramica</span><span class="sxs-lookup"><span data-stu-id="64f05-106">Overview</span></span>

<span data-ttu-id="64f05-107">Per usare l'API Web in un'applicazione Web Form, esistono due passaggi principali:</span><span class="sxs-lookup"><span data-stu-id="64f05-107">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="64f05-108">Aggiungere un controller API Web da cui deriva il **ApiController** classe.</span><span class="sxs-lookup"><span data-stu-id="64f05-108">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="64f05-109">Aggiungere una tabella di route per il **Application\_avviare** (metodo).</span><span class="sxs-lookup"><span data-stu-id="64f05-109">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="64f05-110">Creare un progetto di Web Form</span><span class="sxs-lookup"><span data-stu-id="64f05-110">Create a Web Forms Project</span></span>

<span data-ttu-id="64f05-111">Avviare Visual Studio e selezionare **nuovo progetto** dalle **avviare** pagina.</span><span class="sxs-lookup"><span data-stu-id="64f05-111">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="64f05-112">E viceversa, il **File** dal menu **New** e quindi **progetto**.</span><span class="sxs-lookup"><span data-stu-id="64f05-112">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="64f05-113">Nel **modelli** riquadro, selezionare **modelli installati** ed espandere le **Visual c#** nodo.</span><span class="sxs-lookup"><span data-stu-id="64f05-113">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="64f05-114">Sotto **Visual c#**, selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="64f05-114">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="64f05-115">Nell'elenco dei modelli di progetto, selezionare **applicazione Web Form ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="64f05-115">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="64f05-116">Immettere un nome per il progetto e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="64f05-116">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="64f05-117">Creare il modello e Controller</span><span class="sxs-lookup"><span data-stu-id="64f05-117">Create the Model and Controller</span></span>

<span data-ttu-id="64f05-118">Questa esercitazione Usa le stesse classi del modello e controller come il [introduttiva](tutorial-your-first-web-api.md) esercitazione.</span><span class="sxs-lookup"><span data-stu-id="64f05-118">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="64f05-119">In primo luogo, aggiungere una classe di modello.</span><span class="sxs-lookup"><span data-stu-id="64f05-119">First, add a model class.</span></span> <span data-ttu-id="64f05-120">Nelle **Esplora soluzioni**, fare clic sul progetto e selezionare **Aggiungi classe**.</span><span class="sxs-lookup"><span data-stu-id="64f05-120">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="64f05-121">Denominare la classe Product e aggiungere l'implementazione seguente:</span><span class="sxs-lookup"><span data-stu-id="64f05-121">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="64f05-122">Successivamente, aggiungere un controller API Web al progetto., una *controller* è l'oggetto che gestisce le richieste HTTP per l'API Web.</span><span class="sxs-lookup"><span data-stu-id="64f05-122">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="64f05-123">Nelle **Esplora soluzioni**, fare clic sul progetto.</span><span class="sxs-lookup"><span data-stu-id="64f05-123">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="64f05-124">Selezionare **Aggiungi nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="64f05-124">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="64f05-125">Sotto **modelli installati**, espandere **Visual c#** e selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="64f05-125">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="64f05-126">Quindi, nell'elenco dei modelli, selezionare **classe Controller API Web**.</span><span class="sxs-lookup"><span data-stu-id="64f05-126">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="64f05-127">Denominare il controller "ProductsController" e fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="64f05-127">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="64f05-128">Il **Aggiungi nuovo elemento** procedura guidata creerà un file denominato ProductsController.cs.</span><span class="sxs-lookup"><span data-stu-id="64f05-128">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="64f05-129">Eliminare i metodi che è inclusa la procedura guidata e aggiungere i metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="64f05-129">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="64f05-130">Per altre informazioni sul codice in questo controller, vedere la [introduttiva](tutorial-your-first-web-api.md) esercitazione.</span><span class="sxs-lookup"><span data-stu-id="64f05-130">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="64f05-131">Aggiungere le informazioni di Routing</span><span class="sxs-lookup"><span data-stu-id="64f05-131">Add Routing Information</span></span>

<span data-ttu-id="64f05-132">Successivamente, si aggiungerà una route URI in modo che gli URI nel formato &quot;/API/prodotti/&quot; vengono indirizzati al controller.</span><span class="sxs-lookup"><span data-stu-id="64f05-132">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="64f05-133">Nelle **Esplora soluzioni**, fare doppio clic su Global. asax per aprire il file code-behind Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="64f05-133">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="64f05-134">Aggiungere il codice seguente **usando** istruzione.</span><span class="sxs-lookup"><span data-stu-id="64f05-134">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="64f05-135">Quindi aggiungere il codice seguente per il **Application\_avviare** metodo:</span><span class="sxs-lookup"><span data-stu-id="64f05-135">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="64f05-136">Per altre informazioni sulle tabelle di routing, vedere [Routing in API Web ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="64f05-136">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="64f05-137">Aggiungere AJAX lato Client</span><span class="sxs-lookup"><span data-stu-id="64f05-137">Add Client-Side AJAX</span></span>

<span data-ttu-id="64f05-138">Questo è tutto che è necessario creare un'API web che i client possono accedere.</span><span class="sxs-lookup"><span data-stu-id="64f05-138">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="64f05-139">A questo punto è possibile aggiungere una pagina HTML che usa jQuery per chiamare l'API.</span><span class="sxs-lookup"><span data-stu-id="64f05-139">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="64f05-140">Assicurarsi che la pagina master (ad esempio, *Site. master*) include una `ContentPlaceHolder` con `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="64f05-140">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="64f05-141">Aprire il file default. aspx.</span><span class="sxs-lookup"><span data-stu-id="64f05-141">Open the file Default.aspx.</span></span> <span data-ttu-id="64f05-142">Sostituire il testo boilerplate nella sezione del contenuto principale, come illustrato:</span><span class="sxs-lookup"><span data-stu-id="64f05-142">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="64f05-143">Successivamente, aggiungere un riferimento al file di origine jQuery nel `HeaderContent` sezione:</span><span class="sxs-lookup"><span data-stu-id="64f05-143">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="64f05-144">Nota: È possibile facilmente aggiungere il riferimento allo script trascinando e rilasciando il file dalla **Esplora soluzioni** nella finestra dell'editor di codice.</span><span class="sxs-lookup"><span data-stu-id="64f05-144">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="64f05-145">Sotto il tag di script jQuery, aggiungere il seguente blocco di script:</span><span class="sxs-lookup"><span data-stu-id="64f05-145">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="64f05-146">Quando il documento viene caricato, lo script esegue una richiesta AJAX &quot;api/prodotti&quot;.</span><span class="sxs-lookup"><span data-stu-id="64f05-146">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="64f05-147">La richiesta restituisce un elenco di prodotti in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="64f05-147">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="64f05-148">Lo script aggiunge le informazioni sul prodotto per la tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="64f05-148">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="64f05-149">Quando si esegue l'applicazione, dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="64f05-149">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
