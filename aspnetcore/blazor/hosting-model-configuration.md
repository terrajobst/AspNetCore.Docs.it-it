---
title: ASP.NET Core Blazor configurazione del modello di hosting
author: guardrex
description: Informazioni su Blazor la configurazione del modello di hosting, inclusa la procedura per integrare i componenti Razor in app Razor Pages e MVC.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/24/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: 1f71ac63bbe9dc9d56cfca2ded19a5b863be828f
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306430"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="0f2d9-103">Configurazione del modello di hosting di ASP.NET Core Blazer</span><span class="sxs-lookup"><span data-stu-id="0f2d9-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="0f2d9-104">Di [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="0f2d9-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="0f2d9-105">Questo articolo illustra la configurazione del modello di hosting.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-105">This article covers hosting model configuration.</span></span>

## <a name="blazor-webassembly"></a><span data-ttu-id="0f2d9-106">WebAssembly Blazor</span><span class="sxs-lookup"><span data-stu-id="0f2d9-106">Blazor WebAssembly</span></span>

<span data-ttu-id="0f2d9-107">A partire dalla versione di ASP.NET Core 3,2 Preview 3, il webassembly Blazer supporta la configurazione da:</span><span class="sxs-lookup"><span data-stu-id="0f2d9-107">As of the ASP.NET Core 3.2 Preview 3 release, Blazor WebAssembly supports configuration from:</span></span>

* <span data-ttu-id="0f2d9-108">*Wwwroot/appSettings. JSON*</span><span class="sxs-lookup"><span data-stu-id="0f2d9-108">*wwwroot/appsettings.json*</span></span>
* <span data-ttu-id="0f2d9-109">*Wwwroot/appSettings. {ENVIRONMENT}. JSON*</span><span class="sxs-lookup"><span data-stu-id="0f2d9-109">*wwwroot/appsettings.{ENVIRONMENT}.json*</span></span>

<span data-ttu-id="0f2d9-110">In un'app ospitata da Blazer, l' [ambiente di runtime](xref:fundamentals/environments) è uguale al valore dell'app Server.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-110">In a Blazor Hosted app, the [runtime environment](xref:fundamentals/environments) is the same as the server app's value.</span></span>

<span data-ttu-id="0f2d9-111">Quando si esegue l'app in locale, l'ambiente USA per impostazione predefinita lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-111">When running the app locally, the environment defaults to Development.</span></span> <span data-ttu-id="0f2d9-112">Quando l'app viene pubblicata, per impostazione predefinita viene impostato l'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-112">When the app is published, the environment defaults to Production.</span></span> <span data-ttu-id="0f2d9-113">Per ulteriori informazioni, tra cui come configurare l'ambiente, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-113">For more information, including how to configure the environment, see <xref:fundamentals/environments>.</span></span>

> [!WARNING]
> <span data-ttu-id="0f2d9-114">La configurazione in un'app webassembly blazer è visibile agli utenti.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-114">Configuration in a Blazor WebAssembly app is visible to users.</span></span> <span data-ttu-id="0f2d9-115">**Non archiviare i segreti dell'app o le credenziali nella configurazione.**</span><span class="sxs-lookup"><span data-stu-id="0f2d9-115">**Don't store app secrets or credentials in configuration.**</span></span>

<span data-ttu-id="0f2d9-116">I file di configurazione vengono memorizzati nella cache per l'uso offline.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-116">Configuration files are cached for offline use.</span></span> <span data-ttu-id="0f2d9-117">Con [le applicazioni Web progressive (PWA)](xref:blazor/progressive-web-app)è possibile aggiornare solo i file di configurazione quando si crea una nuova distribuzione.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-117">With [Progressive Web Applications (PWAs)](xref:blazor/progressive-web-app), you can only update configuration files when creating a new deployment.</span></span> <span data-ttu-id="0f2d9-118">La modifica dei file di configurazione tra le distribuzioni non ha effetto perché:</span><span class="sxs-lookup"><span data-stu-id="0f2d9-118">Editing configuration files between deployments has no effect because:</span></span>

* <span data-ttu-id="0f2d9-119">Gli utenti dispongono di versioni memorizzate nella cache dei file che continuano a usare.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-119">Users have cached versions of the files that they continue to use.</span></span>
* <span data-ttu-id="0f2d9-120">I file *service-worker. js* e *service-worker-assets. js* di PWA devono essere ricompilati durante la compilazione, che segnalano all'app la prossima visita online dell'utente che l'app è stata ridistribuita.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-120">The PWA's *service-worker.js* and *service-worker-assets.js* files must be rebuilt on compilation, which signal to the app on the user's next online visit that the app has been redeployed.</span></span>

<span data-ttu-id="0f2d9-121">Per ulteriori informazioni sulla gestione degli aggiornamenti in background da PWA, vedere <xref:blazor/progressive-web-app#background-updates>.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-121">For more information on how background updates are handled by PWAs, see <xref:blazor/progressive-web-app#background-updates>.</span></span>

## <a name="blazor-server"></a><span data-ttu-id="0f2d9-122">Server Blazor</span><span class="sxs-lookup"><span data-stu-id="0f2d9-122">Blazor Server</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="0f2d9-123">Riflette lo stato di connessione nell'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="0f2d9-123">Reflect the connection state in the UI</span></span>

<span data-ttu-id="0f2d9-124">Quando il client rileva che la connessione è stata persa, viene visualizzata un'interfaccia utente predefinita quando il client tenta di riconnettersi.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-124">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="0f2d9-125">Se la riconnessione non riesce, all'utente viene offerta l'opzione per riprovare.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-125">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="0f2d9-126">Per personalizzare l'interfaccia utente, definire un elemento con un `id` di `components-reconnect-modal` nel `<body>` della pagina Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="0f2d9-126">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="0f2d9-127">Nella tabella seguente vengono descritte le classi CSS applicate all'elemento `components-reconnect-modal`.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-127">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="0f2d9-128">Classe CSS</span><span class="sxs-lookup"><span data-stu-id="0f2d9-128">CSS class</span></span>                       | <span data-ttu-id="0f2d9-129">Indica&hellip;</span><span class="sxs-lookup"><span data-stu-id="0f2d9-129">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="0f2d9-130">Connessione persa.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-130">A lost connection.</span></span> <span data-ttu-id="0f2d9-131">È in corso un tentativo di riconnessione del client.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-131">The client is attempting to reconnect.</span></span> <span data-ttu-id="0f2d9-132">Mostra il modale.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-132">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="0f2d9-133">Viene ristabilita una connessione attiva al server.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-133">An active connection is re-established to the server.</span></span> <span data-ttu-id="0f2d9-134">Nascondere il modale.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-134">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="0f2d9-135">Riconnessione non riuscita. probabilmente a causa di un errore di rete.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-135">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="0f2d9-136">Per tentare la riconnessione, chiamare `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-136">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="0f2d9-137">Riconnessione rifiutata.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-137">Reconnection rejected.</span></span> <span data-ttu-id="0f2d9-138">Il server è stato raggiunto ma ha rifiutato la connessione e lo stato dell'utente nel server è andato perso.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-138">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="0f2d9-139">Per ricaricare l'app, chiamare `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-139">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="0f2d9-140">Questo stato di connessione può verificarsi nei casi seguenti:</span><span class="sxs-lookup"><span data-stu-id="0f2d9-140">This connection state may result when:</span></span><ul><li><span data-ttu-id="0f2d9-141">Si verifica un arresto anomalo del circuito sul lato server.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-141">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="0f2d9-142">Il client viene disconnesso abbastanza a lungo da consentire al server di eliminare lo stato dell'utente.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-142">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="0f2d9-143">Le istanze dei componenti con cui l'utente interagisce sono state eliminate.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-143">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="0f2d9-144">Il server viene riavviato oppure il processo di lavoro dell'app viene riciclato.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-144">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="0f2d9-145">Modalità di rendering</span><span class="sxs-lookup"><span data-stu-id="0f2d9-145">Render mode</span></span>

<span data-ttu-id="0f2d9-146">Per impostazione predefinita, le app del server Blazor sono configurate per eseguire il prerendering dell'interfaccia utente nel server prima che venga stabilita la connessione client al server.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-146">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="0f2d9-147">Questa impostazione è configurata nella pagina Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="0f2d9-147">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="0f2d9-148">`RenderMode` configura se il componente:</span><span class="sxs-lookup"><span data-stu-id="0f2d9-148">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="0f2d9-149">Viene preeseguito nella pagina.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-149">Is prerendered into the page.</span></span>
* <span data-ttu-id="0f2d9-150">Viene visualizzato come HTML statico nella pagina o se include le informazioni necessarie per eseguire il bootstrap di un'app Blazor dall'agente utente.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-150">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="0f2d9-151">Descrizione</span><span class="sxs-lookup"><span data-stu-id="0f2d9-151">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="0f2d9-152">Esegue il rendering del componente in HTML statico e include un marcatore per un'app Server Blazor.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-152">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="0f2d9-153">Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-153">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="0f2d9-154">Esegue il rendering di un marcatore per un'app Server Blazor.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-154">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="0f2d9-155">L'output del componente non è incluso.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-155">Output from the component isn't included.</span></span> <span data-ttu-id="0f2d9-156">Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-156">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="0f2d9-157">Esegue il rendering del componente in HTML statico.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-157">Renders the component into static HTML.</span></span> |

<span data-ttu-id="0f2d9-158">Il rendering dei componenti server da una pagina HTML statica non è supportato.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-158">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="0f2d9-159">Eseguire il rendering di componenti interattivi con stato da pagine e visualizzazioni Razor</span><span class="sxs-lookup"><span data-stu-id="0f2d9-159">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="0f2d9-160">I componenti interattivi con stato possono essere aggiunti a una pagina o a una visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-160">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="0f2d9-161">Quando viene eseguito il rendering della pagina o della visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="0f2d9-161">When the page or view renders:</span></span>

* <span data-ttu-id="0f2d9-162">Il componente viene preeseguito con la pagina o la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-162">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="0f2d9-163">Lo stato iniziale del componente utilizzato per il prerendering viene perso.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-163">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="0f2d9-164">Quando viene stabilita la connessione SignalR viene creato il nuovo stato del componente.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-164">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="0f2d9-165">La pagina Razor seguente esegue il rendering di un componente `Counter`:</span><span class="sxs-lookup"><span data-stu-id="0f2d9-165">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="0f2d9-166">Eseguire il rendering di componenti non interattivi da pagine e visualizzazioni Razor</span><span class="sxs-lookup"><span data-stu-id="0f2d9-166">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="0f2d9-167">Nella pagina Razor seguente il componente `Counter` viene sottoposto a rendering statico con un valore iniziale specificato utilizzando un form:</span><span class="sxs-lookup"><span data-stu-id="0f2d9-167">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

<component type="typeof(Counter)" render-mode="Static" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

<span data-ttu-id="0f2d9-168">Poiché `MyComponent` viene sottoposto a rendering statico, il componente non può essere interattivo.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-168">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="0f2d9-169">Configurare il client di SignalR per le app Blazor server</span><span class="sxs-lookup"><span data-stu-id="0f2d9-169">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="0f2d9-170">In alcuni casi, è necessario configurare il client SignalR usato dalle app Blazor server.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-170">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="0f2d9-171">È ad esempio possibile configurare la registrazione nel client di SignalR per diagnosticare un problema di connessione.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-171">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="0f2d9-172">Per configurare il client di SignalR nel file *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="0f2d9-172">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="0f2d9-173">Aggiungere un attributo `autostart="false"` al tag `<script>` per lo script `blazor.server.js`.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-173">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="0f2d9-174">Chiamare `Blazor.start` e passare un oggetto di configurazione che specifichi il generatore di SignalR.</span><span class="sxs-lookup"><span data-stu-id="0f2d9-174">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging("information"); // LogLevel.Information
    }
  });
</script>
```
