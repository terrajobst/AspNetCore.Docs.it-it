---
title: Collegamento del browser in ASP.NET Core
author: ncarandini
description: "Viene spiegato come collegamento del Browser è una funzionalità di Visual Studio che collega l'ambiente di sviluppo con uno o più web browser."
ms.author: riande
manager: wpickett
ms.date: 09/22/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-browserlink
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d5db65c268923e96c45b034639437fc3496ccac1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="c523e-103">Collegamento del browser in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c523e-103">Browser Link in ASP.NET Core</span></span> 

<span data-ttu-id="c523e-104">Da [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c523e-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="c523e-105">Collegamento del browser è una funzionalità di Visual Studio che crea un canale di comunicazione tra l'ambiente di sviluppo e uno o più web browser.</span><span class="sxs-lookup"><span data-stu-id="c523e-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="c523e-106">È possibile utilizzare il collegamento del Browser per aggiornare l'applicazione web nel browser diverse contemporaneamente, che è utile per il testing e browser.</span><span class="sxs-lookup"><span data-stu-id="c523e-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="c523e-107">Installazione del collegamento browser</span><span class="sxs-lookup"><span data-stu-id="c523e-107">Browser Link setup</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c523e-108">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c523e-108">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c523e-109">ASP.NET Core 2. x **applicazione Web**, **vuoto**, e **API Web** modello progetti utilizzano il [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) Meta pacchetto, che contiene un riferimento al pacchetto per [browserlink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span><span class="sxs-lookup"><span data-stu-id="c523e-109">The ASP.NET Core 2.x **Web Application**, **Empty**, and **Web API** template projects use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) meta-package, which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="c523e-110">Pertanto, l'utilizzo di `Microsoft.AspNetCore.All` meta pacchetto non richiede alcuna azione aggiuntiva per rendere disponibili per l'utilizzo di collegamento del Browser.</span><span class="sxs-lookup"><span data-stu-id="c523e-110">Therefore, using the `Microsoft.AspNetCore.All` meta-package requires no further action to make Browser Link available for use.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c523e-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c523e-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c523e-112">ASP.NET Core 1. x **applicazione Web** modello di progetto è un riferimento al pacchetto per il [browserlink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="c523e-112">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="c523e-113">Il **vuoto** o **API Web** progetti di modello è necessario aggiungere un riferimento pacchetto `Microsoft.VisualStudio.Web.BrowserLink`.</span><span class="sxs-lookup"><span data-stu-id="c523e-113">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="c523e-114">Poiché si tratta di una funzionalità di Visual Studio, il modo più semplice per aggiungere il pacchetto a un **vuoto** o **API Web** progetto modello è anche possibile aprire il **Package Manager Console** (**Visualizzazione** > **altre finestre** > **Package Manager Console**) ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c523e-114">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="c523e-115">In alternativa, è possibile utilizzare **Gestione pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c523e-115">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="c523e-116">Fare doppio clic sul nome del progetto in **Esplora** e scegliere **Gestisci pacchetti NuGet**:</span><span class="sxs-lookup"><span data-stu-id="c523e-116">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![Aprire Gestisci pacchetti NuGet](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="c523e-118">Trovare e installare il pacchetto:</span><span class="sxs-lookup"><span data-stu-id="c523e-118">Find and install the package:</span></span>

![Aggiungere il pacchetto con Gestione pacchetti NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

---

### <a name="configuration"></a><span data-ttu-id="c523e-120">Configurazione</span><span class="sxs-lookup"><span data-stu-id="c523e-120">Configuration</span></span>

<span data-ttu-id="c523e-121">Nel `Configure` metodo il *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="c523e-121">In the `Configure` method of the *Startup.cs* file:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="c523e-122">In genere il codice è all'interno di un `if` blocco che consente solo di collegamento del Browser nell'ambiente di sviluppo, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="c523e-122">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="c523e-123">Per altre informazioni, vedere [Uso di più ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="c523e-123">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="c523e-124">Come utilizzare il collegamento del Browser</span><span class="sxs-lookup"><span data-stu-id="c523e-124">How to use Browser Link</span></span>

<span data-ttu-id="c523e-125">Quando è aperto un progetto ASP.NET Core, Visual Studio Mostra il controllo barra degli strumenti di collegamento del Browser accanto al **destinazione di Debug** controllo barra degli strumenti:</span><span class="sxs-lookup"><span data-stu-id="c523e-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Menu a discesa collegamento browser](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="c523e-127">Dal controllo della barra degli strumenti di collegamento del Browser, è possibile:</span><span class="sxs-lookup"><span data-stu-id="c523e-127">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="c523e-128">Aggiornare l'applicazione web nel browser diverse contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="c523e-128">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="c523e-129">Aprire il **Dashboard del collegamento Browser**.</span><span class="sxs-lookup"><span data-stu-id="c523e-129">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="c523e-130">Abilitare o disabilitare **collegamento Browser**.</span><span class="sxs-lookup"><span data-stu-id="c523e-130">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="c523e-131">Nota: Collegamento del Browser è disabilitato per impostazione predefinita in Visual Studio 2017 (15.3).</span><span class="sxs-lookup"><span data-stu-id="c523e-131">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="c523e-132">Abilitare o disabilitare [sincronizzazione automatica CSS](#enable-or-disable-css-auto-sync).</span><span class="sxs-lookup"><span data-stu-id="c523e-132">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="c523e-133">Alcuni plug-in Visual Studio, in particolare *Web estensione Pack 2015* e *Web estensione Pack 2017*, offrono funzionalità estese per il collegamento del Browser, ma alcune funzionalità aggiuntive non funzionano con ASP. Progetti di componenti di base NET.</span><span class="sxs-lookup"><span data-stu-id="c523e-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a><span data-ttu-id="c523e-134">Aggiornare l'applicazione web browser diversi in una sola volta</span><span class="sxs-lookup"><span data-stu-id="c523e-134">Refresh the web application in several browsers at once</span></span>

<span data-ttu-id="c523e-135">Per scegliere un singolo web browser per avviare all'avvio del progetto, utilizzare il menu di scelta rapida nel **destinazione di Debug** controllo barra degli strumenti:</span><span class="sxs-lookup"><span data-stu-id="c523e-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![Menu di scelta rapida F5](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="c523e-137">Per aprire contemporaneamente più browser, scegliere **Esplora con...**  dalla stessa elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="c523e-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="c523e-138">Tenere premuto il tasto CTRL per selezionare i browser desiderato e quindi fare clic su **Sfoglia**:</span><span class="sxs-lookup"><span data-stu-id="c523e-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Aprire contemporaneamente molti browser](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="c523e-140">Ecco una schermata che illustra di Visual Studio con la visualizzazione dell'indice aperto e due i browser aperti:</span><span class="sxs-lookup"><span data-stu-id="c523e-140">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![Sincronizzazione con un esempio di due browser](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="c523e-142">Passare il mouse sopra il controllo barra degli strumenti di collegamento del Browser per visualizzare i browser che sono connessi al progetto:</span><span class="sxs-lookup"><span data-stu-id="c523e-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Suggerimento al passaggio del mouse](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="c523e-144">Modificare la visualizzazione dell'indice e tutti i browser connessi vengono aggiornati quando si fa clic sul pulsante Aggiorna collegamento Browser:</span><span class="sxs-lookup"><span data-stu-id="c523e-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![browsers-sync-to-changes](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="c523e-146">Collegamento del browser funziona anche con i browser che si avvia dall'esterno di Visual Studio e accedere all'URL dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c523e-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="c523e-147">Dashboard del collegamento Browser</span><span class="sxs-lookup"><span data-stu-id="c523e-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="c523e-148">Aprire il Dashboard di collegamento Browser dal menu per gestire la connessione con i browser aperte a discesa collegamento Browser:</span><span class="sxs-lookup"><span data-stu-id="c523e-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![open-browserslink-dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="c523e-150">Se non è connesso alcun browser, è possibile avviare una sessione di debug non selezionando il *Visualizza nel Browser* collegamento:</span><span class="sxs-lookup"><span data-stu-id="c523e-150">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![browserlink-dashboard-no-connections](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="c523e-152">In caso contrario, i browser connessi sono mostrati con il percorso della pagina da cui viene visualizzata ogni browser:</span><span class="sxs-lookup"><span data-stu-id="c523e-152">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-dashboard-two-connections](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="c523e-154">Se si desidera, è possibile fare clic sul nome di un browser elencati per aggiornare il browser singolo.</span><span class="sxs-lookup"><span data-stu-id="c523e-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="c523e-155">Abilitare o disabilitare il collegamento del Browser</span><span class="sxs-lookup"><span data-stu-id="c523e-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="c523e-156">Quando si riabilita collegamento del Browser dopo la disabilitazione, è necessario aggiornare il browser per riconnettersi a essi.</span><span class="sxs-lookup"><span data-stu-id="c523e-156">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="c523e-157">Abilitare o disabilitare la sincronizzazione automatica CSS</span><span class="sxs-lookup"><span data-stu-id="c523e-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="c523e-158">Quando è abilitata sincronizzazione automatica CSS, i browser connessi vengono aggiornati automaticamente quando si apporta modifiche al file CSS.</span><span class="sxs-lookup"><span data-stu-id="c523e-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-does-it-work"></a><span data-ttu-id="c523e-159">Funzionamento</span><span class="sxs-lookup"><span data-stu-id="c523e-159">How does it work?</span></span>

<span data-ttu-id="c523e-160">Collegamento del browser utilizza SignalR per creare un canale di comunicazione tra Visual Studio e il browser.</span><span class="sxs-lookup"><span data-stu-id="c523e-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="c523e-161">Quando il collegamento del Browser è abilitato, Visual Studio funziona come un server di SignalR in grado di connettersi a più client (browser).</span><span class="sxs-lookup"><span data-stu-id="c523e-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="c523e-162">Collegamento del browser registra anche un componente del middleware nella pipeline delle richieste ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c523e-162">Browser Link also registers a middleware component in the ASP.NET request pipeline.</span></span> <span data-ttu-id="c523e-163">Il componente inserisce speciali `<script>` riferimenti in ogni richiesta di pagina dal server.</span><span class="sxs-lookup"><span data-stu-id="c523e-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="c523e-164">È possibile visualizzare i riferimenti a script selezionando **Visualizza origine** nel browser e lo scorrimento alla fine del `<body>` tag al contenuto:</span><span class="sxs-lookup"><span data-stu-id="c523e-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="c523e-165">I file di origine non vengono modificati.</span><span class="sxs-lookup"><span data-stu-id="c523e-165">Your source files aren't modified.</span></span> <span data-ttu-id="c523e-166">Il componente middleware inserisce i riferimenti a script in modo dinamico.</span><span class="sxs-lookup"><span data-stu-id="c523e-166">The middleware component injects the script references dynamically.</span></span> 

<span data-ttu-id="c523e-167">Dal momento che il codice lato browser è il supporto di JavaScript, funziona per tutti i browser che supporta SignalR senza richiedere un plug-in del browser.</span><span class="sxs-lookup"><span data-stu-id="c523e-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
