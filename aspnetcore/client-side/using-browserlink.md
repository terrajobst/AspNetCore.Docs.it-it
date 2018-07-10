---
title: Collegamento del browser in ASP.NET Core
author: ncarandini
description: Viene illustrato come il collegamento del Browser è una funzionalità di Visual Studio che si collega l'ambiente di sviluppo con uno o più web browser.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
uid: client-side/using-browserlink
ms.openlocfilehash: 5ab15c841c472e6c9d47bad70fcf5e0c6dc3010f
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894179"
---
# <a name="browser-link-in-aspnet-core"></a><span data-ttu-id="3c148-103">Collegamento del browser in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3c148-103">Browser Link in ASP.NET Core</span></span>

<span data-ttu-id="3c148-104">Dal [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="3c148-104">By [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="3c148-105">Collegamento del browser è una funzionalità di Visual Studio che crea un canale di comunicazione tra l'ambiente di sviluppo e uno o più web browser.</span><span class="sxs-lookup"><span data-stu-id="3c148-105">Browser Link is a feature in Visual Studio that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="3c148-106">È possibile usare il collegamento del Browser per aggiornare l'applicazione web in diversi browser in una sola volta, è utile per i test tra browser.</span><span class="sxs-lookup"><span data-stu-id="3c148-106">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

## <a name="browser-link-setup"></a><span data-ttu-id="3c148-107">Programma di installazione di browser Link</span><span class="sxs-lookup"><span data-stu-id="3c148-107">Browser Link setup</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="3c148-108">Durante la conversione di un progetto ASP.NET Core 2.0 in ASP.NET Core 2.1 e in fase di transizione per il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), installare il [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) creare un pacchetto per Funzionalità BrowserLink.</span><span class="sxs-lookup"><span data-stu-id="3c148-108">When converting an ASP.NET Core 2.0 project to ASP.NET Core 2.1 and transitioning to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), install the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package for BrowserLink functionality.</span></span> <span data-ttu-id="3c148-109">Usano i modelli di progetto ASP.NET Core 2.1 il `Microsoft.AspNetCore.App` metapacchetto per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="3c148-109">The ASP.NET Core 2.1 project templates use the `Microsoft.AspNetCore.App` metapackage by default.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="3c148-110">ASP.NET Core 2.0 **applicazione Web**, **vuota**, e **API Web** utilizzo di modelli di progetto di [metapacchetto Microsoft. aspnetcore](xref:fundamentals/metapackage) , che contiene un riferimento al pacchetto per [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span><span class="sxs-lookup"><span data-stu-id="3c148-110">The ASP.NET Core 2.0 **Web Application**, **Empty**, and **Web API** project templates use the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which contains a package reference for [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/).</span></span> <span data-ttu-id="3c148-111">Pertanto, l'utilizzo di `Microsoft.AspNetCore.All` metapacchetto non richiede alcuna azione aggiuntiva per rendere disponibili per l'uso di collegamento del Browser.</span><span class="sxs-lookup"><span data-stu-id="3c148-111">Therefore, using the `Microsoft.AspNetCore.All` metapackage requires no further action to make Browser Link available for use.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="3c148-112">ASP.NET Core 1.x **applicazione Web** modello di progetto contiene un riferimento al pacchetto per il [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="3c148-112">The ASP.NET Core 1.x **Web Application** project template has a package reference for the [Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) package.</span></span> <span data-ttu-id="3c148-113">Il **vuote** oppure **API Web** progetti di modello richiedono è quindi necessario aggiungere un riferimento al pacchetto `Microsoft.VisualStudio.Web.BrowserLink`.</span><span class="sxs-lookup"><span data-stu-id="3c148-113">The **Empty** or **Web API** template projects require you to add a package reference to `Microsoft.VisualStudio.Web.BrowserLink`.</span></span>

<span data-ttu-id="3c148-114">Poiché si tratta di una funzionalità di Visual Studio, il modo più semplice per aggiungere il pacchetto a un **vuota** o **API Web** progetto del modello consiste nell'aprire il **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="3c148-114">Since this is a Visual Studio feature, the easiest way to add the package to an **Empty** or **Web API** template project is to open the **Package Manager Console** (**View** > **Other Windows** > **Package Manager Console**) and run the following command:</span></span>

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

<span data-ttu-id="3c148-115">In alternativa, è possibile usare **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3c148-115">Alternatively, you can use **NuGet Package Manager**.</span></span> <span data-ttu-id="3c148-116">Fare clic sul nome del progetto in **Esplora soluzioni** e scegliere **Gestisci pacchetti NuGet**:</span><span class="sxs-lookup"><span data-stu-id="3c148-116">Right-click the project name in **Solution Explorer** and choose **Manage NuGet Packages**:</span></span>

![Gestione pacchetti NuGet Open](using-browserlink/_static/open-nuget-package-manager.png)

<span data-ttu-id="3c148-118">Trovare e installare il pacchetto:</span><span class="sxs-lookup"><span data-stu-id="3c148-118">Find and install the package:</span></span>

![Aggiungere il pacchetto con Gestione pacchetti NuGet](using-browserlink/_static/add-package-with-nuget-package-manager.png)

::: moniker-end

### <a name="configuration"></a><span data-ttu-id="3c148-120">Configurazione</span><span class="sxs-lookup"><span data-stu-id="3c148-120">Configuration</span></span>

<span data-ttu-id="3c148-121">Nel metodo `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="3c148-121">In the `Startup.Configure` method:</span></span>

```csharp
app.UseBrowserLink();
```

<span data-ttu-id="3c148-122">È in genere il codice all'interno di un `if` blocco che consente solo il collegamento del Browser nell'ambiente di sviluppo, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="3c148-122">Usually the code is inside an `if` block that only enables Browser Link in the Development environment, as shown here:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

<span data-ttu-id="3c148-123">Per altre informazioni, vedere [Usare più ambienti](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="3c148-123">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="how-to-use-browser-link"></a><span data-ttu-id="3c148-124">Come usare il collegamento del Browser</span><span class="sxs-lookup"><span data-stu-id="3c148-124">How to use Browser Link</span></span>

<span data-ttu-id="3c148-125">Quando è aperto un progetto ASP.NET Core, Visual Studio visualizza il controllo di collegamento del Browser sulla barra degli strumenti accanto al **destinazione di Debug** toolbar (controllo):</span><span class="sxs-lookup"><span data-stu-id="3c148-125">When you have an ASP.NET Core project open, Visual Studio shows the Browser Link toolbar control next to the **Debug Target** toolbar control:</span></span>

![Menu a discesa collegamento browser](using-browserlink/_static/browserLink-dropdown-menu.png)

<span data-ttu-id="3c148-127">Dal controllo della barra degli strumenti di collegamento del Browser, è possibile:</span><span class="sxs-lookup"><span data-stu-id="3c148-127">From the Browser Link toolbar control, you can:</span></span>

* <span data-ttu-id="3c148-128">Aggiornare l'applicazione web in diversi browser in una sola volta.</span><span class="sxs-lookup"><span data-stu-id="3c148-128">Refresh the web application in several browsers at once.</span></span>
* <span data-ttu-id="3c148-129">Aprire il **Dashboard Browser Link**.</span><span class="sxs-lookup"><span data-stu-id="3c148-129">Open the **Browser Link Dashboard**.</span></span>
* <span data-ttu-id="3c148-130">Abilitare o disabilitare **collegamento del Browser**.</span><span class="sxs-lookup"><span data-stu-id="3c148-130">Enable or disable **Browser Link**.</span></span> <span data-ttu-id="3c148-131">Nota: Il collegamento del Browser è disabilitato per impostazione predefinita in Visual Studio 2017 (versione 15.3).</span><span class="sxs-lookup"><span data-stu-id="3c148-131">Note: Browser Link is disabled by default in Visual Studio 2017 (15.3).</span></span>
* <span data-ttu-id="3c148-132">Abilitare o disabilitare [sincronizzazione automatica CSS](#enable-or-disable-css-auto-sync).</span><span class="sxs-lookup"><span data-stu-id="3c148-132">Enable or disable [CSS Auto-Sync](#enable-or-disable-css-auto-sync).</span></span>

> [!NOTE]
> <span data-ttu-id="3c148-133">Alcuni plug-in Visual Studio, in particolare *Web estensione Pack 2015* e *Web estensione Pack 2017*, offrono funzionalità estese per il collegamento del Browser, ma alcune funzionalità aggiuntive non funzionano con ASP. Progetti .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3c148-133">Some Visual Studio plug-ins, most notably *Web Extension Pack 2015* and *Web Extension Pack 2017*, offer extended functionality for Browser Link, but some of the additional features don't work with ASP.NET Core projects.</span></span>

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a><span data-ttu-id="3c148-134">Aggiornare l'app web in diversi browser in una sola volta</span><span class="sxs-lookup"><span data-stu-id="3c148-134">Refresh the web app in several browsers at once</span></span>

<span data-ttu-id="3c148-135">Per scegliere un browser web singola da avviare quando si avvia il progetto, usare il menu a discesa nel **destinazione di Debug** toolbar (controllo):</span><span class="sxs-lookup"><span data-stu-id="3c148-135">To choose a single web browser to launch when starting the project, use the drop-down menu in the **Debug Target** toolbar control:</span></span>

![Menu di scelta rapida F5](using-browserlink/_static/debug-target-dropdown-menu.png)

<span data-ttu-id="3c148-137">Per aprire più browser in una sola volta, scegliere **Esplora con...**  dalla stessa elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="3c148-137">To open multiple browsers at once, choose **Browse with...** from the same drop-down.</span></span> <span data-ttu-id="3c148-138">Tenere premuto il tasto CTRL per selezionare il browser desiderato e quindi fare clic su **esplorare**:</span><span class="sxs-lookup"><span data-stu-id="3c148-138">Hold down the CTRL key to select the browsers you want, and then click **Browse**:</span></span>

![Apri molti browser in una sola volta](using-browserlink/_static/open-many-browsers-at-once.png)

<span data-ttu-id="3c148-140">Ecco uno screenshot che mostra Visual Studio con la visualizzazione dell'indice open e due i browser aperti:</span><span class="sxs-lookup"><span data-stu-id="3c148-140">Here's a screenshot showing Visual Studio with the Index view open and two open browsers:</span></span>

![La sincronizzazione con esempio di due browser](using-browserlink/_static/sync-with-two-browsers-example.png)

<span data-ttu-id="3c148-142">Passare il mouse sopra il controllo della barra degli strumenti di collegamento del Browser per visualizzare i browser connessi al progetto:</span><span class="sxs-lookup"><span data-stu-id="3c148-142">Hover over the Browser Link toolbar control to see the browsers that are connected to the project:</span></span>

![Suggerimento al passaggio del mouse](using-browserlink/_static/hoover-tip.png)

<span data-ttu-id="3c148-144">Modificare la visualizzazione dell'indice e tutti i browser collegati vengono aggiornati quando si fa clic sul pulsante Aggiorna il collegamento del Browser:</span><span class="sxs-lookup"><span data-stu-id="3c148-144">Change the Index view, and all connected browsers are updated when you click the Browser Link refresh button:</span></span>

![browser-sync-a-changes](using-browserlink/_static/browsers-sync-to-changes.png)

<span data-ttu-id="3c148-146">Collegamento del browser funziona anche con i browser che avvia da esterno a Visual Studio e passare all'URL dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3c148-146">Browser Link also works with browsers that you launch from outside Visual Studio and navigate to the application URL.</span></span>

### <a name="the-browser-link-dashboard"></a><span data-ttu-id="3c148-147">Dashboard Browser Link</span><span class="sxs-lookup"><span data-stu-id="3c148-147">The Browser Link Dashboard</span></span>

<span data-ttu-id="3c148-148">Aprire il Dashboard Browser Link nell'elenco di Browser Link a discesa di menu per gestire la connessione con il browser aperto:</span><span class="sxs-lookup"><span data-stu-id="3c148-148">Open the Browser Link Dashboard from the Browser Link drop down menu to manage the connection with open browsers:</span></span>

![open-browserslink-dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

<span data-ttu-id="3c148-150">Se non è connesso alcun browser, è possibile avviare una sessione di debug non selezionando il *Visualizza nel Browser* collegamento:</span><span class="sxs-lookup"><span data-stu-id="3c148-150">If no browser is connected, you can start a non-debugging session by selecting the *View in Browser* link:</span></span>

![browserlink-dashboard-no-connessioni](using-browserlink/_static/browserlink-dashboard-no-connections.png)

<span data-ttu-id="3c148-152">In caso contrario, il browser connessi vengono visualizzati con il percorso della pagina che mostra ogni browser:</span><span class="sxs-lookup"><span data-stu-id="3c148-152">Otherwise, the connected browsers are shown with the path to the page that each browser is showing:</span></span>

![browserlink-dashboard-2-connessioni](using-browserlink/_static/browserlink-dashboard-two-connections.png)

<span data-ttu-id="3c148-154">Se si desidera, è possibile fare clic sul nome di un browser elencati per aggiornare il browser singolo.</span><span class="sxs-lookup"><span data-stu-id="3c148-154">If you like, you can click on a listed browser name to refresh that single browser.</span></span>

### <a name="enable-or-disable-browser-link"></a><span data-ttu-id="3c148-155">Abilitare o disabilitare il collegamento del Browser</span><span class="sxs-lookup"><span data-stu-id="3c148-155">Enable or disable Browser Link</span></span>

<span data-ttu-id="3c148-156">Quando si riabilita il collegamento del Browser dopo la sua disabilitazione, è necessario aggiornare il browser per riconnettersi a essi.</span><span class="sxs-lookup"><span data-stu-id="3c148-156">When you re-enable Browser Link after disabling it, you must refresh the browsers to reconnect them.</span></span>

### <a name="enable-or-disable-css-auto-sync"></a><span data-ttu-id="3c148-157">Abilitare o disabilitare la sincronizzazione automatica CSS</span><span class="sxs-lookup"><span data-stu-id="3c148-157">Enable or disable CSS Auto-Sync</span></span>

<span data-ttu-id="3c148-158">Quando è abilitato sincronizzazione automatica CSS, browser connessi vengono aggiornati automaticamente quando si apportano modifiche al file CSS.</span><span class="sxs-lookup"><span data-stu-id="3c148-158">When CSS Auto-Sync is enabled, connected browsers are automatically refreshed when you make any change to CSS files.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="3c148-159">Come funziona</span><span class="sxs-lookup"><span data-stu-id="3c148-159">How it works</span></span>

<span data-ttu-id="3c148-160">Collegamento del browser Usa SignalR per creare un canale di comunicazione tra Visual Studio e il browser.</span><span class="sxs-lookup"><span data-stu-id="3c148-160">Browser Link uses SignalR to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="3c148-161">Quando è abilitato il collegamento del Browser, Visual Studio funge da un server di SignalR in grado di connettersi a più client (browser).</span><span class="sxs-lookup"><span data-stu-id="3c148-161">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="3c148-162">Collegamento del browser registra anche un componente del middleware nella pipeline delle richieste ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3c148-162">Browser Link also registers a middleware component in the ASP.NET request pipeline.</span></span> <span data-ttu-id="3c148-163">Il componente inserisce speciali `<script>` riferimenti in ogni richiesta di pagina dal server.</span><span class="sxs-lookup"><span data-stu-id="3c148-163">This component injects special `<script>` references into every page request from the server.</span></span> <span data-ttu-id="3c148-164">È possibile visualizzare i riferimenti a script selezionando **Visualizza origine** nel browser e lo scorrimento al fine del `<body>` contrassegnare il contenuto:</span><span class="sxs-lookup"><span data-stu-id="3c148-164">You can see the script references by selecting **View source** in the browser and scrolling to the end of the `<body>` tag content:</span></span>

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

<span data-ttu-id="3c148-165">I file di origine non verranno più modificati.</span><span class="sxs-lookup"><span data-stu-id="3c148-165">Your source files aren't modified.</span></span> <span data-ttu-id="3c148-166">Il componente del middleware inserisce in modo dinamico i riferimenti a script.</span><span class="sxs-lookup"><span data-stu-id="3c148-166">The middleware component injects the script references dynamically.</span></span>

<span data-ttu-id="3c148-167">Dal momento che il codice del browser-side è il supporto di JavaScript, funziona in tutti i browser che supporta SignalR senza richiedere un plug-in del browser.</span><span class="sxs-lookup"><span data-stu-id="3c148-167">Because the browser-side code is all JavaScript, it works on all browsers that SignalR supports without requiring a browser plug-in.</span></span>
