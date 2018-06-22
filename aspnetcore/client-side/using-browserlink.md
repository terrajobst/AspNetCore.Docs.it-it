---
title: Collegamento del browser in ASP.NET Core
author: ncarandini
description: Viene spiegato come collegamento del Browser è una funzionalità di Visual Studio che collega l'ambiente di sviluppo con uno o più web browser.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
uid: client-side/using-browserlink
ms.openlocfilehash: 8808dc705ec87ebf6e7874ad69616ed5bbf61576
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274092"
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="d9533-103">Collegamento del browser in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d9533-103">Browser Link in ASP.NET Core</span></span>

<span data-ttu-id="d9533-104">Dal [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d9533-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="d9533-105">Collegamento del browser è una funzionalità di Visual Studio che crea un canale di comunicazione tra l'ambiente di sviluppo e uno o più web browser.</span><span class="sxs-lookup"><span data-stu-id="d9533-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="d9533-106">È possibile utilizzare il collegamento del Browser per aggiornare l'applicazione web nel browser diverse contemporaneamente, che è utile per il testing e browser.</span><span class="sxs-lookup"><span data-stu-id="d9533-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="d9533-107">Installazione del collegamento browser</span><span class="sxs-lookup"><span data-stu-id="d9533-107">Browser Link setup</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d9533-108">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d9533-108">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d9533-109">ASP.NET Core 2.0 **applicazione Web**, **vuota**, e **API Web** modello progetti utilizzano il [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage , che contiene un riferimento al pacchetto per [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span><span class="sxs-lookup"><span data-stu-id="d9533-109">The ASP.NET Core 2.0 **Web Application**, **Empty**, and **Web API** template projects use the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="d9533-110">Pertanto, si utilizza il `Microsoft.AspNetCore.All` metapackage è richiesta alcuna azione aggiuntiva per rendere disponibili ai fini del collegamento del Browser.</span><span class="sxs-lookup"><span data-stu-id="d9533-110">Therefore, using the `Microsoft.AspNetCore.All` metapackage requires no further action to make Browser Link available for use.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d9533-111">Quando si converte un progetto ASP.NET Core 2.0 in ASP.NET Core 2.1 e in fase di transizione per il [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) metapackage, è necessario installare il [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) pacchetto manualmente per BrowserLink funzionalità.</span><span class="sxs-lookup"><span data-stu-id="d9533-111">When converting an ASP.NET Core 2.0 project to ASP.NET Core 2.1 and transitioning to the [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) metapackage, you must install the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package manually for BrowserLink functionality.</span></span>

::: moniker-end

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d9533-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d9533-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d9533-113">ASP.NET Core 1. x **applicazione Web** modello di progetto è un riferimento al pacchetto per il [browserlink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="d9533-113">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="d9533-114">Il **vuoto** o **API Web** progetti di modello è necessario aggiungere un riferimento pacchetto `Microsoft.VisualStudio.Web.BrowserLink`.</span><span class="sxs-lookup"><span data-stu-id="d9533-114">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="d9533-115">Poiché si tratta di una funzionalità di Visual Studio, il modo più semplice per aggiungere il pacchetto a un **vuoto** o **API Web** progetto modello è anche possibile aprire il **Package Manager Console** (**Visualizzazione** > **altre finestre** > **Package Manager Console**) ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="d9533-115">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="d9533-116">In alternativa, è possibile utilizzare **Gestione pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="d9533-116">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="d9533-117">Fare doppio clic sul nome del progetto in **Esplora** e scegliere **Gestisci pacchetti NuGet**:</span><span class="sxs-lookup"><span data-stu-id="d9533-117">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![Aprire Gestisci pacchetti NuGet](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="d9533-119">Trovare e installare il pacchetto:</span><span class="sxs-lookup"><span data-stu-id="d9533-119">Find and install the package:</span></span>

![Aggiungere il pacchetto con Gestione pacchetti NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

---

### <a name="configuration"></a><span data-ttu-id="d9533-121">Configurazione</span><span class="sxs-lookup"><span data-stu-id="d9533-121">Configuration</span></span>

<span data-ttu-id="d9533-122">Nel `Configure` metodo il *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="d9533-122">In the `Configure` method of the *Startup.cs* file:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="d9533-123">In genere il codice è all'interno di un `if` blocco che consente solo di collegamento del Browser nell'ambiente di sviluppo, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="d9533-123">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="d9533-124">Per altre informazioni, vedere [Usare più ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="d9533-124">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="d9533-125">Come utilizzare il collegamento del Browser</span><span class="sxs-lookup"><span data-stu-id="d9533-125">How to use Browser Link</span></span>

<span data-ttu-id="d9533-126">Quando è aperto un progetto ASP.NET Core, Visual Studio Mostra il controllo barra degli strumenti di collegamento del Browser accanto al **destinazione di Debug** controllo barra degli strumenti:</span><span class="sxs-lookup"><span data-stu-id="d9533-126">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Menu a discesa collegamento browser](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="d9533-128">Dal controllo della barra degli strumenti di collegamento del Browser, è possibile:</span><span class="sxs-lookup"><span data-stu-id="d9533-128">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="d9533-129">Aggiornare l'applicazione web nel browser diverse contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="d9533-129">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="d9533-130">Aprire il **Dashboard del collegamento Browser**.</span><span class="sxs-lookup"><span data-stu-id="d9533-130">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="d9533-131">Abilitare o disabilitare **collegamento Browser**.</span><span class="sxs-lookup"><span data-stu-id="d9533-131">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="d9533-132">Nota: Collegamento del Browser è disabilitato per impostazione predefinita in Visual Studio 2017 (15.3).</span><span class="sxs-lookup"><span data-stu-id="d9533-132">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="d9533-133">Abilitare o disabilitare [sincronizzazione automatica CSS](#enable-or-disable-css-auto-sync).</span><span class="sxs-lookup"><span data-stu-id="d9533-133">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="d9533-134">Alcuni plug-in Visual Studio, in particolare *Web estensione Pack 2015* e *Web estensione Pack 2017*, offrono funzionalità estese per il collegamento del Browser, ma alcune funzionalità aggiuntive non funzionano con ASP. Progetti di componenti di base NET.</span><span class="sxs-lookup"><span data-stu-id="d9533-134">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a><span data-ttu-id="d9533-135">Aggiornare l'applicazione web browser diversi in una sola volta</span><span class="sxs-lookup"><span data-stu-id="d9533-135">Refresh the web application in several browsers at once</span></span>

<span data-ttu-id="d9533-136">Per scegliere un singolo web browser per avviare all'avvio del progetto, utilizzare il menu di scelta rapida nel **destinazione di Debug** controllo barra degli strumenti:</span><span class="sxs-lookup"><span data-stu-id="d9533-136">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![Menu di scelta rapida F5](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="d9533-138">Per aprire contemporaneamente più browser, scegliere **Esplora con...**  dalla stessa elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="d9533-138">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="d9533-139">Tenere premuto il tasto CTRL per selezionare i browser desiderato e quindi fare clic su **Sfoglia**:</span><span class="sxs-lookup"><span data-stu-id="d9533-139">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Aprire contemporaneamente molti browser](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="d9533-141">Ecco una schermata che illustra di Visual Studio con la visualizzazione dell'indice aperto e due i browser aperti:</span><span class="sxs-lookup"><span data-stu-id="d9533-141">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![Sincronizzazione con un esempio di due browser](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="d9533-143">Passare il mouse sopra il controllo barra degli strumenti di collegamento del Browser per visualizzare i browser che sono connessi al progetto:</span><span class="sxs-lookup"><span data-stu-id="d9533-143">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Suggerimento al passaggio del mouse](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="d9533-145">Modificare la visualizzazione dell'indice e tutti i browser connessi vengono aggiornati quando si fa clic sul pulsante Aggiorna collegamento Browser:</span><span class="sxs-lookup"><span data-stu-id="d9533-145">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![browser sincronizzazione di modifiche](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="d9533-147">Collegamento del browser funziona anche con i browser che si avvia dall'esterno di Visual Studio e accedere all'URL dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d9533-147">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="d9533-148">Dashboard del collegamento Browser</span><span class="sxs-lookup"><span data-stu-id="d9533-148">The Browser Link Dashboard</span></span>

<span data-ttu-id="d9533-149">Aprire il Dashboard di collegamento Browser dal menu per gestire la connessione con i browser aperte a discesa collegamento Browser:</span><span class="sxs-lookup"><span data-stu-id="d9533-149">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![open-browserslink-dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="d9533-151">Se non è connesso alcun browser, è possibile avviare una sessione di debug non selezionando il *Visualizza nel Browser* collegamento:</span><span class="sxs-lookup"><span data-stu-id="d9533-151">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![browserlink-dashboard-no-connessioni](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="d9533-153">In caso contrario, i browser connessi sono mostrati con il percorso della pagina da cui viene visualizzata ogni browser:</span><span class="sxs-lookup"><span data-stu-id="d9533-153">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-dashboard-due connessioni](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="d9533-155">Se si desidera, è possibile fare clic sul nome di un browser elencati per aggiornare il browser singolo.</span><span class="sxs-lookup"><span data-stu-id="d9533-155">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="d9533-156">Abilitare o disabilitare il collegamento del Browser</span><span class="sxs-lookup"><span data-stu-id="d9533-156">Enable or disable Browser Link</span></span>

<span data-ttu-id="d9533-157">Quando si riabilita collegamento del Browser dopo la disabilitazione, è necessario aggiornare il browser per riconnettersi a essi.</span><span class="sxs-lookup"><span data-stu-id="d9533-157">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="d9533-158">Abilitare o disabilitare la sincronizzazione automatica CSS</span><span class="sxs-lookup"><span data-stu-id="d9533-158">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="d9533-159">Quando è abilitata sincronizzazione automatica CSS, i browser connessi vengono aggiornati automaticamente quando si apporta modifiche al file CSS.</span><span class="sxs-lookup"><span data-stu-id="d9533-159">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-does-it-work"></a><span data-ttu-id="d9533-160">Funzionamento</span><span class="sxs-lookup"><span data-stu-id="d9533-160">How does it work?</span></span>

<span data-ttu-id="d9533-161">Collegamento del browser utilizza SignalR per creare un canale di comunicazione tra Visual Studio e il browser.</span><span class="sxs-lookup"><span data-stu-id="d9533-161">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="d9533-162">Quando il collegamento del Browser è abilitato, Visual Studio funziona come un server di SignalR in grado di connettersi a più client (browser).</span><span class="sxs-lookup"><span data-stu-id="d9533-162">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="d9533-163">Collegamento del browser registra anche un componente del middleware nella pipeline delle richieste ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d9533-163">Browser Link also registers a middleware component in the ASP.NET request pipeline.</span></span> <span data-ttu-id="d9533-164">Il componente inserisce speciali `<script>` riferimenti in ogni richiesta di pagina dal server.</span><span class="sxs-lookup"><span data-stu-id="d9533-164">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="d9533-165">È possibile visualizzare i riferimenti a script selezionando **Visualizza origine** nel browser e lo scorrimento alla fine del `<body>` tag al contenuto:</span><span class="sxs-lookup"><span data-stu-id="d9533-165">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="d9533-166">I file di origine non vengono modificati.</span><span class="sxs-lookup"><span data-stu-id="d9533-166">Your source files aren't modified.</span></span> <span data-ttu-id="d9533-167">Il componente middleware inserisce i riferimenti a script in modo dinamico.</span><span class="sxs-lookup"><span data-stu-id="d9533-167">The middleware component injects the script references dynamically.</span></span> 

<span data-ttu-id="d9533-168">Dal momento che il codice lato browser è il supporto di JavaScript, funziona per tutti i browser che supporta SignalR senza richiedere un plug-in del browser.</span><span class="sxs-lookup"><span data-stu-id="d9533-168">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
