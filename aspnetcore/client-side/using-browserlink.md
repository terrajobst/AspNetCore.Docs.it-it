---
title: Collegamento del browser in ASP.NET Core
author: ncarandini
description: "Una funzionalità di Visual Studio che collega l'ambiente di sviluppo con uno o più web browser"
keywords: ASP.NET Core, il collegamento del browser, sincronizzazione CSS
ms.author: riande
manager: wpickett
ms.date: 12/28/2016
ms.topic: article
ms.assetid: 11813d4c-3f8a-445a-b23b-e4a57d001abc
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-browserlink
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b2ff38288cee3e9ca42a07c219521bb79a00a359
ms.sourcegitcommit: 4e84d8bf5f404bb77f3d41665cf7e7374fc39142
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/05/2017
---
# <a name="introduction-to-browser-link-in-aspnet-core"></a><span data-ttu-id="463ae-104">Introduzione al collegamento del Browser ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="463ae-104">Introduction to Browser Link in ASP.NET Core</span></span> 

<span data-ttu-id="463ae-105">Da [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="463ae-105">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="463ae-106">Collegamento del browser è una funzionalità di Visual Studio che crea un canale di comunicazione tra l'ambiente di sviluppo e uno o più web browser.</span><span class="sxs-lookup"><span data-stu-id="463ae-106">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="463ae-107">È possibile utilizzare il collegamento del Browser per aggiornare l'applicazione web nel browser diverse contemporaneamente, che è utile per il testing e browser.</span><span class="sxs-lookup"><span data-stu-id="463ae-107">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="463ae-108">Installazione del collegamento browser</span><span class="sxs-lookup"><span data-stu-id="463ae-108">Browser Link setup</span></span>

<span data-ttu-id="463ae-109">ASP.NET Core **applicazione Web** modelli di progetto nella finestra di Visual Studio 2015 e versioni successive sono disponibili tutti gli elementi necessari per il collegamento del Browser.</span><span class="sxs-lookup"><span data-stu-id="463ae-109">The ASP.NET Core **Web Application** project templates in Visual Studio 2015 and later include everything needed for Browser Link.</span></span>

<span data-ttu-id="463ae-110">Per aggiungere il collegamento del Browser a un progetto creato utilizzando ASP.NET Core **vuoto** o **API Web** modello, seguire questi passaggi:</span><span class="sxs-lookup"><span data-stu-id="463ae-110">To add Browser Link to a project that you created by using the ASP.NET Core **Empty** or **Web API** template, follow these steps:</span></span>

1. <span data-ttu-id="463ae-111">Aggiungere il *Microsoft.VisualStudio.Web.BrowserLink.Loader* pacchetto</span><span class="sxs-lookup"><span data-stu-id="463ae-111">Add the *Microsoft.VisualStudio.Web.BrowserLink.Loader* package</span></span> 
2. <span data-ttu-id="463ae-112">Aggiungere il codice di configurazione nel *Startup.cs* file.</span><span class="sxs-lookup"><span data-stu-id="463ae-112">Add configuration code in the *Startup.cs* file.</span></span>

### <a name="add-the-package"></a><span data-ttu-id="463ae-113">Aggiungere il pacchetto</span><span class="sxs-lookup"><span data-stu-id="463ae-113">Add the package</span></span>

<span data-ttu-id="463ae-114">Poiché si tratta di una funzionalità di Visual Studio, il modo più semplice per aggiungere il pacchetto è per aprire la **Package Manager Console** (**Vista > altre finestre > Console di gestione pacchetti**) ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="463ae-114">Since this is a Visual Studio feature, the easiest way to add the package is to open the **Package Manager Console** (**View > Other Windows > Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink.Loader
```

<span data-ttu-id="463ae-115">In alternativa, è possibile utilizzare **Gestione pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="463ae-115">Alternatively, you can use **NuGet Package Manager**.</span></span>  <span data-ttu-id="463ae-116">Fare doppio clic sul nome del progetto in **Esplora**e scegliere **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="463ae-116">Right-click the project name in **Solution Explorer**, and choose **Manage NuGet Packages**.</span></span> 

![Aprire Gestisci pacchetti NuGet](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="463ae-118">Quindi, individuare e installare il pacchetto.</span><span class="sxs-lookup"><span data-stu-id="463ae-118">Then find and install the package.</span></span>

![Aggiungere il pacchetto con Gestione pacchetti NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

### <a name="add-configuration-code"></a><span data-ttu-id="463ae-120">Aggiungere il codice di configurazione</span><span class="sxs-lookup"><span data-stu-id="463ae-120">Add configuration code</span></span>

<span data-ttu-id="463ae-121">Aprire il *Startup.cs* file e il `Configure` metodo aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="463ae-121">Open the *Startup.cs* file, and in the `Configure` method add the following code:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="463ae-122">In genere che il codice è all'interno di un `if` blocco che consente il collegamento Browser solo nell'ambiente di sviluppo, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="463ae-122">Usually that code is inside an `if` block that enables Browser Link only in the Development environment, as shown here:</span></span>

<span data-ttu-id="463ae-123">[!code-csharp[Principale](./using-browserlink/sample/BrowserLinkSample/src/BrowserLinkSample/Startup.cs?highlight=1,4&range=40-44)]</span><span class="sxs-lookup"><span data-stu-id="463ae-123">[!code-csharp[Main](./using-browserlink/sample/BrowserLinkSample/src/BrowserLinkSample/Startup.cs?highlight=1,4&range=40-44)]</span></span>

<span data-ttu-id="463ae-124">Per ulteriori informazioni, vedere [utilizzo di più ambienti](../fundamentals/environments.md).</span><span class="sxs-lookup"><span data-stu-id="463ae-124">For more information, see [Working with Multiple Environments](../fundamentals/environments.md).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="463ae-125">Come utilizzare il collegamento del Browser</span><span class="sxs-lookup"><span data-stu-id="463ae-125">How to use Browser Link</span></span>

<span data-ttu-id="463ae-126">Quando è aperto un progetto ASP.NET Core, Visual Studio Mostra il controllo barra degli strumenti di collegamento del Browser accanto al **destinazione di Debug** controllo barra degli strumenti:</span><span class="sxs-lookup"><span data-stu-id="463ae-126">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Menu a discesa collegamento browser](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="463ae-128">Dal controllo della barra degli strumenti di collegamento del Browser, è possibile:</span><span class="sxs-lookup"><span data-stu-id="463ae-128">From the Browser Link toolbar control, you can:</span></span>

- <span data-ttu-id="463ae-129">Aggiornare l'applicazione web browser diversi in una sola volta</span><span class="sxs-lookup"><span data-stu-id="463ae-129">Refresh the web application in several browsers at once</span></span>
- <span data-ttu-id="463ae-130">Aprire il **Dashboard del collegamento Browser**</span><span class="sxs-lookup"><span data-stu-id="463ae-130">Open the **Browser Link Dashboard**</span></span>
- <span data-ttu-id="463ae-131">Abilitare o disabilitare **collegamento Browser**</span><span class="sxs-lookup"><span data-stu-id="463ae-131">Enable or disable **Browser Link**</span></span>
- <span data-ttu-id="463ae-132">Abilitare o disabilitare la sincronizzazione automatica CSS</span><span class="sxs-lookup"><span data-stu-id="463ae-132">Enable or disable CSS Auto-Sync</span></span>

> [!NOTE]
> <span data-ttu-id="463ae-133">Alcuni plug-in Visual Studio, in particolare *Web estensione Pack 2015* e *Web estensione Pack 2017*, offrono funzionalità estese per il collegamento del Browser, ma alcune funzionalità aggiuntive non funzionano con ASP. Progetti di componenti di base NET.</span><span class="sxs-lookup"><span data-stu-id="463ae-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a><span data-ttu-id="463ae-134">Aggiornare l'applicazione web browser diversi in una sola volta</span><span class="sxs-lookup"><span data-stu-id="463ae-134">Refresh the web application in several browsers at once</span></span>

<span data-ttu-id="463ae-135">Per scegliere un singolo web browser per avviare all'avvio del progetto, utilizzare il menu di scelta rapida nel **destinazione di Debug** controllo barra degli strumenti:</span><span class="sxs-lookup"><span data-stu-id="463ae-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![Menu di scelta rapida F5](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="463ae-137">Per aprire contemporaneamente più browser, scegliere **Esplora con...**  dalla stessa elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="463ae-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span>  <span data-ttu-id="463ae-138">Tenere premuto il tasto CTRL per selezionare i browser desiderato e quindi fare clic su **Sfoglia**:</span><span class="sxs-lookup"><span data-stu-id="463ae-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Aprire contemporaneamente molti browser](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="463ae-140">Ecco una schermata di esempio con Visual Studio con la visualizzazione dell'indice aperto e due i browser aperti:</span><span class="sxs-lookup"><span data-stu-id="463ae-140">Here's a sample screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![Sincronizzazione con un esempio di due browser](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="463ae-142">Passare il mouse sopra il controllo barra degli strumenti di collegamento del Browser per visualizzare i browser che sono connessi al progetto:</span><span class="sxs-lookup"><span data-stu-id="463ae-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Suggerimento al passaggio del mouse](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="463ae-144">Modificare la visualizzazione dell'indice e tutti i browser connessi vengono aggiornati quando si fa clic sul pulsante Aggiorna collegamento Browser:</span><span class="sxs-lookup"><span data-stu-id="463ae-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![browser sincronizzazione di modifiche](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="463ae-146">Collegamento del browser funziona anche con i browser che si avvia dall'esterno di Visual Studio e accedere all'URL dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="463ae-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="463ae-147">Dashboard del collegamento Browser</span><span class="sxs-lookup"><span data-stu-id="463ae-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="463ae-148">Aprire il Dashboard di collegamento Browser dal menu per gestire la connessione con i browser aperte a discesa collegamento Browser:</span><span class="sxs-lookup"><span data-stu-id="463ae-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![dashboard di browserslink Apri](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="463ae-150">Se non è connesso alcun browser, è possibile avviare una sessione non debug scegliendo il _Visualizza nel Browser_ collegamento:</span><span class="sxs-lookup"><span data-stu-id="463ae-150">If no browser is connected, you can start a non debugging session clicking the _View in Browser_ link:</span></span>

![browserlink-dashboard-no-connessioni](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="463ae-152">In caso contrario, vengono visualizzati i browser connessi, con il percorso della pagina da cui viene visualizzata ogni browser:</span><span class="sxs-lookup"><span data-stu-id="463ae-152">Otherwise, the connected browsers are shown, with the path to the page that each browser is showing:</span></span>

![browserlink-dashboard-due connessioni](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="463ae-154">Se si desidera, è possibile fare clic sul nome di un browser elencati per aggiornare il browser singolo.</span><span class="sxs-lookup"><span data-stu-id="463ae-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="463ae-155">Abilitare o disabilitare il collegamento del Browser</span><span class="sxs-lookup"><span data-stu-id="463ae-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="463ae-156">Quando si riabilita collegamento del Browser dopo la disabilitazione, è necessario aggiornare il browser per riconnettersi a essi.</span><span class="sxs-lookup"><span data-stu-id="463ae-156">When you re-enable Browser Link after disabling it, you have to refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="463ae-157">Abilitare o disabilitare la sincronizzazione automatica CSS</span><span class="sxs-lookup"><span data-stu-id="463ae-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="463ae-158">Quando è abilitata sincronizzazione automatica CSS, i browser connessi vengono aggiornati automaticamente quando si apporta modifiche al file CSS.</span><span class="sxs-lookup"><span data-stu-id="463ae-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-does-it-work"></a><span data-ttu-id="463ae-159">Funzionamento</span><span class="sxs-lookup"><span data-stu-id="463ae-159">How does it work?</span></span>

<span data-ttu-id="463ae-160">Collegamento del browser utilizza SignalR per creare un canale di comunicazione tra Visual Studio e il browser.</span><span class="sxs-lookup"><span data-stu-id="463ae-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="463ae-161">Quando il collegamento del Browser è abilitato, Visual Studio funziona come un server di SignalR in grado di connettersi a più client (browser).</span><span class="sxs-lookup"><span data-stu-id="463ae-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="463ae-162">Collegamento del browser registra anche un componente del middleware nella pipeline delle richieste ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="463ae-162">Browser Link also registers a middleware component in the ASP.NET request pipeline.</span></span> <span data-ttu-id="463ae-163">Il componente inserisce speciali `<script>` riferimenti in ogni richiesta di pagina dal server.</span><span class="sxs-lookup"><span data-stu-id="463ae-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="463ae-164">È possibile visualizzare i riferimenti a script selezionando **Visualizza origine** nel browser e lo scorrimento alla fine del `<body>` tag al contenuto:</span><span class="sxs-lookup"><span data-stu-id="463ae-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="463ae-165">I file di origine non vengono modificati.</span><span class="sxs-lookup"><span data-stu-id="463ae-165">Your source files are not modified.</span></span> <span data-ttu-id="463ae-166">Il componente middleware inserisce i riferimenti a script in modo dinamico.</span><span class="sxs-lookup"><span data-stu-id="463ae-166">The middleware component injects the script references dynamically.</span></span> 

<span data-ttu-id="463ae-167">Dal momento che il codice lato browser è il supporto di JavaScript, funziona per tutti i browser che supporta SignalR, senza richiedere alcun plug-in browser.</span><span class="sxs-lookup"><span data-stu-id="463ae-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports, without requiring any browser plug-in.</span></span>
