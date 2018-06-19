---
uid: visual-studio/overview/2013/using-browser-link
title: Utilizzo di collegamento del Browser in Visual Studio 2013 | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/04/2013
ms.topic: article
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: e5a13405a303580ec8c1d4cdacafc26c6f8ff34a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044110"
---
<a name="using-browser-link-in-visual-studio-2013"></a><span data-ttu-id="d5d1a-102">Utilizzo di collegamento del Browser in Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d5d1a-102">Using Browser Link in Visual Studio 2013</span></span>
====================
<span data-ttu-id="d5d1a-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d5d1a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="d5d1a-104">Collegamento del browser è una nuova funzionalità di Visual Studio 2013 che crea un canale di comunicazione tra l'ambiente di sviluppo e uno o più web browser.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-104">Browser Link is a new feature in Visual Studio 2013 that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="d5d1a-105">È possibile utilizzare il collegamento del Browser per aggiornare l'applicazione web nel browser diverse contemporaneamente, che è utile per il testing e browser.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-105">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

- [<span data-ttu-id="d5d1a-106">Aggiornamento del browser</span><span class="sxs-lookup"><span data-stu-id="d5d1a-106">Browser Refresh</span></span>](#browser-refresh)
- [<span data-ttu-id="d5d1a-107">Visualizzazione Dashboard del collegamento Browser</span><span class="sxs-lookup"><span data-stu-id="d5d1a-107">Viewing the Browser Link Dashboard</span></span>](#dashboard)
- [<span data-ttu-id="d5d1a-108">Abilitando il collegamento del Browser per i file HTML statico</span><span class="sxs-lookup"><span data-stu-id="d5d1a-108">Enabling Browser Link for Static HTML Files</span></span>](#static-html)
- [<span data-ttu-id="d5d1a-109">La disabilitazione di collegamento del Browser</span><span class="sxs-lookup"><span data-stu-id="d5d1a-109">Disabling Browser Link</span></span>](#disabling)
- [<span data-ttu-id="d5d1a-110">Come funziona?</span><span class="sxs-lookup"><span data-stu-id="d5d1a-110">How Does It Work?</span></span>](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a><span data-ttu-id="d5d1a-111">Aggiornamento del browser</span><span class="sxs-lookup"><span data-stu-id="d5d1a-111">Browser Refresh</span></span>

<span data-ttu-id="d5d1a-112">Con l'aggiornamento del Browser, è possibile aggiornare più browser connessi a Visual Studio tramite il collegamento del Browser.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-112">With Browser Refresh, you can refresh multiple browsers that are connected to Visual Studio through Browser Link.</span></span>

<span data-ttu-id="d5d1a-113">Per utilizzare l'aggiornamento del Browser, creare innanzitutto un'applicazione ASP.NET, utilizzando uno dei modelli di progetto.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-113">To use Browser Refresh, first create an ASP.NET application, using any of the project templates.</span></span> <span data-ttu-id="d5d1a-114">Eseguire il debug dell'applicazione premendo F5 o facendo clic sull'icona della freccia sulla barra degli strumenti:</span><span class="sxs-lookup"><span data-stu-id="d5d1a-114">Debug the application by pressing F5 or clicking the arrow icon in the toolbar:</span></span>

![](using-browser-link/_static/image1.png)

<span data-ttu-id="d5d1a-115">È anche possibile utilizzare l'elenco a discesa per selezionare un browser specifico per il debug.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-115">You can also use the dropdown to select a specific browser for debugging.</span></span>

![](using-browser-link/_static/image2.png)

<span data-ttu-id="d5d1a-116">Per eseguire il debug con più browser, selezionare **Esplora con**.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-116">To debug with multiple browsers, select **Browse With**.</span></span> <span data-ttu-id="d5d1a-117">Nel **Esplora con** finestra di dialogo, tenere premuto il tasto CTRL per selezionare più di un browser.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-117">In the **Browse With** dialog, hold down the CTRL key to select more than one browser.</span></span> <span data-ttu-id="d5d1a-118">Fare clic su **Sfoglia** per eseguire il debug con i browser selezionati.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-118">Click **Browse** to debug with the selected browsers.</span></span> <span data-ttu-id="d5d1a-119">Collegamento del browser funziona anche se si avvia un browser dall'esterno di Visual Studio e accedere all'URL dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-119">Browser Link also works if you launch a browser from outside Visual Studio and navigate to the application URL.</span></span>

![](using-browser-link/_static/image3.png)

<span data-ttu-id="d5d1a-120">I controlli collegamento Browser si trovano nell'elenco a discesa con l'icona freccia circolare.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-120">The Browser Link controls are located in the dropdown with the circular arrow icon.</span></span> <span data-ttu-id="d5d1a-121">L'icona freccia è il **aggiornamento** pulsante.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-121">The arrow icon is the **Refresh** button.</span></span>

![](using-browser-link/_static/image4.png)

<span data-ttu-id="d5d1a-122">Per vedere quali browser connessi, posizionare il mouse sopra il **aggiornamento** pulsante durante il debug.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-122">To see which browsers are connected, hover the mouse over the **Refresh** button while debugging.</span></span> <span data-ttu-id="d5d1a-123">I browser connessi vengono visualizzati in una finestra della descrizione comando.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-123">The connected browsers are shown in a ToolTip window.</span></span>

![](using-browser-link/_static/image5.png)

<span data-ttu-id="d5d1a-124">Per aggiornare il browser connesso, fare clic su di **aggiornamento** oppure premere CTRL + ALT + INVIO.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-124">To refresh the connected browsers, click the **Refresh** button or press CTRL+ALT+ENTER.</span></span> <span data-ttu-id="d5d1a-125">Ad esempio, nella schermata seguente mostra un progetto ASP.NET, che è stato creato utilizzando il modello di progetto MVC 5.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-125">For example, the following screenshot shows an ASP.NET project, which I created using the MVC 5 project template.</span></span> <span data-ttu-id="d5d1a-126">È possibile visualizzare l'applicazione in esecuzione nei due browser nella parte superiore.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-126">You can see the application running in two browsers at the top.</span></span> <span data-ttu-id="d5d1a-127">Nella parte inferiore, il progetto è aperto in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-127">At the bottom, the project is open in Visual Studio.</span></span>

![](using-browser-link/_static/image6.png)

<span data-ttu-id="d5d1a-128">In Visual Studio, in seguito alla modifica di &lt;h1&gt; titolo per la home page:</span><span class="sxs-lookup"><span data-stu-id="d5d1a-128">In Visual Studio, I changed the &lt;h1&gt; heading for the home page:</span></span>

![](using-browser-link/_static/image7.png)

<span data-ttu-id="d5d1a-129">Quando fa clic su di **aggiornamento** pulsante, la modifica è presente in entrambe le finestre del browser:</span><span class="sxs-lookup"><span data-stu-id="d5d1a-129">When I clicked the **Refresh** button, the change appeared in both browser windows:</span></span>

![](using-browser-link/_static/image8.png)

<span data-ttu-id="d5d1a-130">**Note**</span><span class="sxs-lookup"><span data-stu-id="d5d1a-130">**Notes**</span></span>

- <span data-ttu-id="d5d1a-131">Per abilitare il collegamento del Browser, impostare `debug=true` nel [ &lt;compilazione&gt; ](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) elemento nel file Web. config del progetto.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-131">To enable Browser Link, set `debug=true` in the [&lt;compilation&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) element in the project's Web.config file.</span></span>
- <span data-ttu-id="d5d1a-132">L'applicazione deve essere in esecuzione in localhost.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-132">The application must be running on localhost.</span></span>
- <span data-ttu-id="d5d1a-133">L'applicazione deve avere come destinazione .NET 4.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-133">The application must target .NET 4.0 or later.</span></span>

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a><span data-ttu-id="d5d1a-134">Visualizzazione Dashboard del collegamento Browser</span><span class="sxs-lookup"><span data-stu-id="d5d1a-134">Viewing the Browser Link Dashboard</span></span>

<span data-ttu-id="d5d1a-135">Il dashboard di collegamento del Browser Mostra informazioni sulle connessioni di collegamento del Browser.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-135">The Browser Link dashboard shows information about the Browser Link connections.</span></span> <span data-ttu-id="d5d1a-136">Per visualizzare il dashboard, selezionare il menu a discesa di collegamento del Browser (la piccola freccia accanto al **aggiornamento** pulsante).</span><span class="sxs-lookup"><span data-stu-id="d5d1a-136">To view the dashboard, select the Browser Link dropdown menu (the small arrow next to the **Refresh** button).</span></span> <span data-ttu-id="d5d1a-137">Quindi fare clic su **Dashboard del collegamento Browser**.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-137">Then click **Browser Link Dashboard**.</span></span>

![](using-browser-link/_static/image9.png)

<span data-ttu-id="d5d1a-138">I dashboard sono elencati i browser connessi e l'URL a cui si è spostato ogni browser.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-138">The dashboard lists the connected Browsers and the URL to which each browser has navigated.</span></span>

![](using-browser-link/_static/image10.png)

<span data-ttu-id="d5d1a-139">Il **prerequisiti** sezione Mostra tutti i passaggi necessari per abilitare il collegamento del Browser per il progetto.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-139">The **Prerequisites** section shows any steps needed to enable Browser Link for that project.</span></span> <span data-ttu-id="d5d1a-140">Ad esempio, nella schermata seguente viene illustrato un progetto in cui "debug" è impostato su false nel file Web. config.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-140">For example, the following screenshot shows a project where "debug" is set to false in the Web.config file.</span></span>

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a><span data-ttu-id="d5d1a-141">Abilitando il collegamento del Browser per i file HTML statico</span><span class="sxs-lookup"><span data-stu-id="d5d1a-141">Enabling Browser Link for Static HTML Files</span></span>

<span data-ttu-id="d5d1a-142">Per abilitare il collegamento del Browser per i file HTML statici, aggiungere quanto segue al file Web. config.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-142">To enable Browser Link for static HTML files, add the following to your Web.config file.</span></span>

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

<span data-ttu-id="d5d1a-143">Per motivi di prestazioni, è possibile rimuovere questa impostazione quando si pubblica il progetto.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-143">For performance reasons, remove this setting when you publish your project.</span></span>

<a id="disabling"></a>
## <a name="disabling-browser-link"></a><span data-ttu-id="d5d1a-144">La disabilitazione di collegamento del Browser</span><span class="sxs-lookup"><span data-stu-id="d5d1a-144">Disabling Browser Link</span></span>

<span data-ttu-id="d5d1a-145">Collegamento del browser è abilitata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-145">Browser Link is enabled by default.</span></span> <span data-ttu-id="d5d1a-146">Esistono diversi modi per disabilitare la funzionalità:</span><span class="sxs-lookup"><span data-stu-id="d5d1a-146">There are several ways to disable it:</span></span>

- <span data-ttu-id="d5d1a-147">Nel menu a discesa collegamento del Browser, deselezionare **abilitare collegamento Browser**.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-147">In the Browser Link dropdown menu, uncheck **Enable Browser Link**.</span></span> 

    ![](using-browser-link/_static/image12.png)
- <span data-ttu-id="d5d1a-148">Nel file Web. config, aggiungere una chiave denominata "vs: EnableBrowserLink" con valore "false" nella sezione appSettings.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-148">In the Web.config file, add a key named "vs:EnableBrowserLink" with the value "false" in the appSettings section.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- <span data-ttu-id="d5d1a-149">Nel file Web. config impostare debug su false.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-149">In the Web.config file, set debug to false.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a><span data-ttu-id="d5d1a-150">Come funziona?</span><span class="sxs-lookup"><span data-stu-id="d5d1a-150">How Does It Work?</span></span>

<span data-ttu-id="d5d1a-151">Collegamento del browser utilizza [SignalR](../../../signalr/index.md) per creare un canale di comunicazione tra Visual Studio e il browser.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-151">Browser Link uses [SignalR](../../../signalr/index.md) to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="d5d1a-152">Quando il collegamento del Browser è abilitato, Visual Studio funziona come un server di SignalR in grado di connettersi a più client (browser).</span><span class="sxs-lookup"><span data-stu-id="d5d1a-152">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="d5d1a-153">Collegamento del browser registra anche un modulo HTTP con ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-153">Browser Link also registers an HTTP module with ASP.NET.</span></span> <span data-ttu-id="d5d1a-154">Questo modulo inserisce speciali &lt;script&gt; riferimenti in ogni richiesta di pagina dal server.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-154">This module injects special &lt;script&gt; references into every page request from the server.</span></span> <span data-ttu-id="d5d1a-155">È possibile visualizzare i riferimenti a script selezionando "HTML" nel browser.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-155">You can see the script references by selecting "View source" in the browser.</span></span>

![](using-browser-link/_static/image13.png)

<span data-ttu-id="d5d1a-156">I file di origine non vengono modificati.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-156">Your source files are not modified.</span></span> <span data-ttu-id="d5d1a-157">Il modulo HTTP inserisce i riferimenti a script in modo dinamico.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-157">The HTTP module injects the script references dynamically.</span></span>

<span data-ttu-id="d5d1a-158">Dal momento che il codice lato browser è il supporto di JavaScript, funziona per tutti i browser che [SignalR supporta](../../../signalr/overview/getting-started/supported-platforms.md), senza richiedere alcun plug-in browser.</span><span class="sxs-lookup"><span data-stu-id="d5d1a-158">Because the browser-side code is all JavaScript, it works on all browsers that [SignalR supports](../../../signalr/overview/getting-started/supported-platforms.md), without requiring any browser plug-in.</span></span>
