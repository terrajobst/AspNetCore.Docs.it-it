---
title: Browser Link in ASP.NET Core
author: ncarandini
description: Viene illustrato come Browser Link è una funzionalità di Visual Studio che collega l'ambiente di sviluppo a uno o più Web browser.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/12/2019
no-loc:
- SignalR
uid: client-side/using-browserlink
ms.openlocfilehash: b21b698d49e72b559cd9cd3753c48a38c99db24d
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962788"
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="aaa42-103">Browser Link in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aaa42-103">Browser Link in ASP.NET Core</span></span>

<span data-ttu-id="aaa42-104">Di [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson)e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="aaa42-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="aaa42-105">Browser Link è una funzionalità di Visual Studio che consente di creare un canale di comunicazione tra l'ambiente di sviluppo e uno o più Web browser.</span><span class="sxs-lookup"><span data-stu-id="aaa42-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="aaa42-106">È possibile utilizzare Browser Link per aggiornare l'applicazione Web in diversi browser contemporaneamente, operazione utile per i test tra browser.</span><span class="sxs-lookup"><span data-stu-id="aaa42-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="aaa42-107">Installazione di Browser Link</span><span class="sxs-lookup"><span data-stu-id="aaa42-107">Browser Link setup</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="aaa42-108">Quando si converte un progetto ASP.NET Core 2,0 in ASP.NET Core 2,1 e si esegue la transizione al [metapacchetto Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), installare il pacchetto [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) per la funzionalità BrowserLink.</span><span class="sxs-lookup"><span data-stu-id="aaa42-108">When converting an ASP.NET Core 2.0 project to ASP.NET Core 2.1 and transitioning to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), install the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package for BrowserLink functionality.</span></span> <span data-ttu-id="aaa42-109">Per impostazione predefinita, i modelli di progetto ASP.NET Core 2,1 usano il metapacchetto `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="aaa42-109">The ASP.NET Core 2.1 project templates use the `Microsoft.AspNetCore.App` metapackage by default.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="aaa42-110">I modelli di progetto **applicazione Web**, **vuoto**e **API Web** di ASP.NET Core 2,0 usano il [metapacchetto Microsoft. AspNetCore. All](xref:fundamentals/metapackage), che contiene un riferimento al pacchetto per [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span><span class="sxs-lookup"><span data-stu-id="aaa42-110">The ASP.NET Core 2.0 **Web Application**, **Empty**, and **Web API** project templates use the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="aaa42-111">Pertanto, l'utilizzo del metapacchetto `Microsoft.AspNetCore.All` non richiede ulteriori azioni per rendere Browser Link disponibili per l'utilizzo.</span><span class="sxs-lookup"><span data-stu-id="aaa42-111">Therefore, using the `Microsoft.AspNetCore.All` metapackage requires no further action to make Browser Link available for use.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="aaa42-112">Il modello di progetto **applicazione Web** ASP.NET Core 1. x include un riferimento al pacchetto per il pacchetto [Microsoft. VisualStudio. Web. BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) .</span><span class="sxs-lookup"><span data-stu-id="aaa42-112">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="aaa42-113">Per i progetti di modelli di **API Web** o **vuoti** è necessario aggiungere un riferimento al pacchetto a `Microsoft.VisualStudio.Web.BrowserLink`.</span><span class="sxs-lookup"><span data-stu-id="aaa42-113">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="aaa42-114">Poiché si tratta di una funzionalità di Visual Studio, il modo più semplice per aggiungere il pacchetto a un progetto di modello di **API Web** o **vuoto** consiste nell'aprire la **console di gestione pacchetti** (**visualizzare** > altre **console di gestione pacchetti**di **Windows** >) ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="aaa42-114">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="aaa42-115">In alternativa, è possibile usare **Gestione pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="aaa42-115">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="aaa42-116">Fare clic con il pulsante destro del mouse sul nome del progetto in **Esplora soluzioni** e scegliere **Gestisci pacchetti NuGet**:</span><span class="sxs-lookup"><span data-stu-id="aaa42-116">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![Apri Gestione pacchetti NuGet](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="aaa42-118">Trovare e installare il pacchetto:</span><span class="sxs-lookup"><span data-stu-id="aaa42-118">Find and install the package:</span></span>

![Aggiungere un pacchetto con gestione pacchetti NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="aaa42-120">Configurazione</span><span class="sxs-lookup"><span data-stu-id="aaa42-120">Configuration</span></span>

<span data-ttu-id="aaa42-121">Nel metodo `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="aaa42-121">In the `Startup.Configure` method:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="aaa42-122">In genere il codice si trova all'interno di un blocco di `if` che Abilita solo Browser Link nell'ambiente di sviluppo, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="aaa42-122">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="aaa42-123">Per altre informazioni, vedere [Usare più ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="aaa42-123">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="aaa42-124">Come usare Browser Link</span><span class="sxs-lookup"><span data-stu-id="aaa42-124">How to use Browser Link</span></span>

<span data-ttu-id="aaa42-125">Quando si dispone di un progetto ASP.NET Core aperto, Visual Studio Mostra il controllo della barra degli strumenti Browser Link accanto al controllo della barra degli strumenti della **destinazione di debug** :</span><span class="sxs-lookup"><span data-stu-id="aaa42-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Menu a discesa Browser Link](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="aaa42-127">Dal controllo Browser Link barra degli strumenti è possibile:</span><span class="sxs-lookup"><span data-stu-id="aaa42-127">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="aaa42-128">Aggiornare l'applicazione Web in diversi browser contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="aaa42-128">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="aaa42-129">Aprire il **Dashboard browser link**.</span><span class="sxs-lookup"><span data-stu-id="aaa42-129">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="aaa42-130">Abilitare o disabilitare **browser link**.</span><span class="sxs-lookup"><span data-stu-id="aaa42-130">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="aaa42-131">Nota: per impostazione predefinita, Browser Link è disabilitato in Visual Studio 2017 (15,3).</span><span class="sxs-lookup"><span data-stu-id="aaa42-131">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="aaa42-132">Abilitare o disabilitare la [sincronizzazione automatica CSS](#enable-or-disable-css-auto-sync).</span><span class="sxs-lookup"><span data-stu-id="aaa42-132">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="aaa42-133">Alcuni plug-in di Visual Studio, in particolare i *pacchetti Web Extension pack 2015* e *web Extension Pack 2017*, offrono funzionalità estese per browser link, ma alcune funzionalità aggiuntive non funzionano con ASP.NET Core progetti.</span><span class="sxs-lookup"><span data-stu-id="aaa42-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a><span data-ttu-id="aaa42-134">Aggiornare l'app Web in più browser contemporaneamente</span><span class="sxs-lookup"><span data-stu-id="aaa42-134">Refresh the web app in several browsers at once</span></span>

<span data-ttu-id="aaa42-135">Per scegliere un singolo Web browser da avviare all'avvio del progetto, usare il menu a discesa nel controllo della barra degli strumenti della **destinazione di debug** :</span><span class="sxs-lookup"><span data-stu-id="aaa42-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![Menu a discesa F5](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="aaa42-137">Per aprire più browser contemporaneamente, scegliere **Sfoglia con...** dallo stesso elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="aaa42-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="aaa42-138">Tenere premuto il tasto CTRL per selezionare i browser desiderati, quindi fare clic su **Sfoglia**:</span><span class="sxs-lookup"><span data-stu-id="aaa42-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Apri molti browser contemporaneamente](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="aaa42-140">Ecco una schermata che mostra Visual Studio con la visualizzazione indice aperta e due browser aperti:</span><span class="sxs-lookup"><span data-stu-id="aaa42-140">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![Esempio di sincronizzazione con due browser](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="aaa42-142">Passare il puntatore del mouse sul controllo della barra degli strumenti Browser Link per visualizzare i browser connessi al progetto:</span><span class="sxs-lookup"><span data-stu-id="aaa42-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Suggerimento per il passaggio del mouse](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="aaa42-144">Modificare la visualizzazione dell'indice e tutti i browser connessi vengono aggiornati quando si fa clic sul pulsante Browser Link Refresh:</span><span class="sxs-lookup"><span data-stu-id="aaa42-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![browser-Sincronizza modifiche](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="aaa42-146">Browser Link funziona anche con i browser che si avviano dall'esterno di Visual Studio e si passa all'URL dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="aaa42-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="aaa42-147">Dashboard Browser Link</span><span class="sxs-lookup"><span data-stu-id="aaa42-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="aaa42-148">Aprire il Dashboard Browser Link dal menu a discesa Browser Link per gestire la connessione con i browser aperti:</span><span class="sxs-lookup"><span data-stu-id="aaa42-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![Apri-browserslink-dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="aaa42-150">Se non è connesso alcun browser, è possibile avviare una sessione non di debug selezionando la *vista nel* collegamento del browser:</span><span class="sxs-lookup"><span data-stu-id="aaa42-150">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![browserlink-dashboard-no-Connections](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="aaa42-152">In caso contrario, i browser connessi vengono visualizzati con il percorso della pagina visualizzata da ogni browser:</span><span class="sxs-lookup"><span data-stu-id="aaa42-152">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-dashboard-due connessioni](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="aaa42-154">Se lo si desidera, è possibile fare clic sul nome di un browser elencato per aggiornare il singolo browser.</span><span class="sxs-lookup"><span data-stu-id="aaa42-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="aaa42-155">Abilitare o disabilitare Browser Link</span><span class="sxs-lookup"><span data-stu-id="aaa42-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="aaa42-156">Quando si riabilita Browser Link dopo averla disabilitata, è necessario aggiornare i browser per riconnetterli.</span><span class="sxs-lookup"><span data-stu-id="aaa42-156">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="aaa42-157">Abilitare o disabilitare la sincronizzazione automatica CSS</span><span class="sxs-lookup"><span data-stu-id="aaa42-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="aaa42-158">Quando è abilitata la sincronizzazione automatica CSS, i browser connessi vengono aggiornati automaticamente quando si apportano modifiche ai file CSS.</span><span class="sxs-lookup"><span data-stu-id="aaa42-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="aaa42-159">Funzionamento</span><span class="sxs-lookup"><span data-stu-id="aaa42-159">How it works</span></span>

<span data-ttu-id="aaa42-160">Browser Link USA SignalR per creare un canale di comunicazione tra Visual Studio e il browser.</span><span class="sxs-lookup"><span data-stu-id="aaa42-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="aaa42-161">Quando Browser Link è abilitato, Visual Studio funge da server SignalR a cui possono connettersi più client (browser).</span><span class="sxs-lookup"><span data-stu-id="aaa42-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="aaa42-162">Browser Link registra anche un componente middleware nella pipeline delle richieste di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aaa42-162">Browser Link also registers a middleware component in the ASP.NET Core request pipeline.</span></span> <span data-ttu-id="aaa42-163">Questo componente inserisce riferimenti speciali `<script>` in ogni richiesta di pagina dal server.</span><span class="sxs-lookup"><span data-stu-id="aaa42-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="aaa42-164">È possibile visualizzare i riferimenti agli script selezionando **Visualizza origine** nel browser e scorrendo fino alla fine del `<body>` contenuto del Tag:</span><span class="sxs-lookup"><span data-stu-id="aaa42-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="aaa42-165">I file di origine non sono stati modificati.</span><span class="sxs-lookup"><span data-stu-id="aaa42-165">Your source files aren't modified.</span></span> <span data-ttu-id="aaa42-166">Il componente middleware inserisce i riferimenti allo script in modo dinamico.</span><span class="sxs-lookup"><span data-stu-id="aaa42-166">The middleware component injects the script references dynamically.</span></span>

<span data-ttu-id="aaa42-167">Poiché il codice sul lato browser è tutto JavaScript, funziona su tutti i browser che SignalR supporta senza richiedere un plug-in del browser.</span><span class="sxs-lookup"><span data-stu-id="aaa42-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
