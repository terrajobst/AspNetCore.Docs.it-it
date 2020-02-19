---
title: ASP.NET Core Blazor configurazione del modello di hosting
author: guardrex
description: Informazioni su Blazor la configurazione del modello di hosting, inclusa la procedura per integrare i componenti Razor in app Razor Pages e MVC.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: bd44643877e45c5b48b0972bcc2f637fbc5d98f2
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447035"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="2f3ad-103">Configurazione del modello di hosting di ASP.NET Core Blazer</span><span class="sxs-lookup"><span data-stu-id="2f3ad-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="2f3ad-104">Di [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="2f3ad-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="2f3ad-105">Questo articolo illustra la configurazione del modello di hosting.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-105">This article covers hosting model configuration.</span></span>

<!-- For future use:

## Blazor WebAssembly

-->

## <a name="blazor-server"></a><span data-ttu-id="2f3ad-106">Server Blazor</span><span class="sxs-lookup"><span data-stu-id="2f3ad-106">Blazor Server</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="2f3ad-107">Riflette lo stato di connessione nell'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="2f3ad-107">Reflect the connection state in the UI</span></span>

<span data-ttu-id="2f3ad-108">Quando il client rileva che la connessione è stata persa, viene visualizzata un'interfaccia utente predefinita quando il client tenta di riconnettersi.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-108">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="2f3ad-109">Se la riconnessione non riesce, all'utente viene offerta l'opzione per riprovare.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-109">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="2f3ad-110">Per personalizzare l'interfaccia utente, definire un elemento con un `id` di `components-reconnect-modal` nel `<body>` della pagina Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2f3ad-110">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="2f3ad-111">Nella tabella seguente vengono descritte le classi CSS applicate all'elemento `components-reconnect-modal`.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-111">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="2f3ad-112">Classe CSS</span><span class="sxs-lookup"><span data-stu-id="2f3ad-112">CSS class</span></span>                       | <span data-ttu-id="2f3ad-113">Indica&hellip;</span><span class="sxs-lookup"><span data-stu-id="2f3ad-113">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="2f3ad-114">Connessione persa.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-114">A lost connection.</span></span> <span data-ttu-id="2f3ad-115">È in corso un tentativo di riconnessione del client.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-115">The client is attempting to reconnect.</span></span> <span data-ttu-id="2f3ad-116">Mostra il modale.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-116">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="2f3ad-117">Viene ristabilita una connessione attiva al server.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-117">An active connection is re-established to the server.</span></span> <span data-ttu-id="2f3ad-118">Nascondere il modale.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-118">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="2f3ad-119">Riconnessione non riuscita. probabilmente a causa di un errore di rete.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-119">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="2f3ad-120">Per tentare la riconnessione, chiamare `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-120">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="2f3ad-121">Riconnessione rifiutata.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-121">Reconnection rejected.</span></span> <span data-ttu-id="2f3ad-122">Il server è stato raggiunto ma ha rifiutato la connessione e lo stato dell'utente nel server è andato perso.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-122">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="2f3ad-123">Per ricaricare l'app, chiamare `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-123">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="2f3ad-124">Questo stato di connessione può verificarsi nei casi seguenti:</span><span class="sxs-lookup"><span data-stu-id="2f3ad-124">This connection state may result when:</span></span><ul><li><span data-ttu-id="2f3ad-125">Si verifica un arresto anomalo del circuito sul lato server.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-125">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="2f3ad-126">Il client viene disconnesso abbastanza a lungo da consentire al server di eliminare lo stato dell'utente.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-126">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="2f3ad-127">Le istanze dei componenti con cui l'utente interagisce sono state eliminate.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-127">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="2f3ad-128">Il server viene riavviato oppure il processo di lavoro dell'app viene riciclato.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-128">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="2f3ad-129">Modalità di rendering</span><span class="sxs-lookup"><span data-stu-id="2f3ad-129">Render mode</span></span>

<span data-ttu-id="2f3ad-130">Per impostazione predefinita, le app del server Blazor sono configurate per eseguire il prerendering dell'interfaccia utente nel server prima che venga stabilita la connessione client al server.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-130">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="2f3ad-131">Questa impostazione è configurata nella pagina Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2f3ad-131">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="2f3ad-132">`RenderMode` configura se il componente:</span><span class="sxs-lookup"><span data-stu-id="2f3ad-132">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="2f3ad-133">Viene preeseguito nella pagina.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-133">Is prerendered into the page.</span></span>
* <span data-ttu-id="2f3ad-134">Viene visualizzato come HTML statico nella pagina o se include le informazioni necessarie per eseguire il bootstrap di un'app Blazor dall'agente utente.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-134">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="2f3ad-135">Description</span><span class="sxs-lookup"><span data-stu-id="2f3ad-135">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="2f3ad-136">Esegue il rendering del componente in HTML statico e include un marcatore per un'app Server Blazor.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-136">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="2f3ad-137">Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-137">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="2f3ad-138">Esegue il rendering di un marcatore per un'app Server Blazor.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-138">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="2f3ad-139">L'output del componente non è incluso.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-139">Output from the component isn't included.</span></span> <span data-ttu-id="2f3ad-140">Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-140">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="2f3ad-141">Esegue il rendering del componente in HTML statico.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-141">Renders the component into static HTML.</span></span> |

<span data-ttu-id="2f3ad-142">Il rendering dei componenti server da una pagina HTML statica non è supportato.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-142">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="2f3ad-143">Eseguire il rendering di componenti interattivi con stato da pagine e visualizzazioni Razor</span><span class="sxs-lookup"><span data-stu-id="2f3ad-143">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="2f3ad-144">I componenti interattivi con stato possono essere aggiunti a una pagina o a una visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-144">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="2f3ad-145">Quando viene eseguito il rendering della pagina o della visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="2f3ad-145">When the page or view renders:</span></span>

* <span data-ttu-id="2f3ad-146">Il componente viene preeseguito con la pagina o la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-146">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="2f3ad-147">Lo stato iniziale del componente utilizzato per il prerendering viene perso.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-147">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="2f3ad-148">Quando viene stabilita la connessione SignalR viene creato il nuovo stato del componente.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-148">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="2f3ad-149">La pagina Razor seguente esegue il rendering di un componente `Counter`:</span><span class="sxs-lookup"><span data-stu-id="2f3ad-149">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="2f3ad-150">Eseguire il rendering di componenti non interattivi da pagine e visualizzazioni Razor</span><span class="sxs-lookup"><span data-stu-id="2f3ad-150">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="2f3ad-151">Nella pagina Razor seguente il componente `Counter` viene sottoposto a rendering statico con un valore iniziale specificato utilizzando un form:</span><span class="sxs-lookup"><span data-stu-id="2f3ad-151">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

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

<span data-ttu-id="2f3ad-152">Poiché `MyComponent` viene sottoposto a rendering statico, il componente non può essere interattivo.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-152">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="2f3ad-153">Configurare il client di SignalR per le app Blazor server</span><span class="sxs-lookup"><span data-stu-id="2f3ad-153">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="2f3ad-154">In alcuni casi, è necessario configurare il client SignalR usato dalle app Blazor server.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-154">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="2f3ad-155">È ad esempio possibile configurare la registrazione nel client di SignalR per diagnosticare un problema di connessione.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-155">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="2f3ad-156">Per configurare il client di SignalR nel file *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2f3ad-156">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="2f3ad-157">Aggiungere un attributo `autostart="false"` al tag `<script>` per lo script `blazor.server.js`.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-157">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="2f3ad-158">Chiamare `Blazor.start` e passare un oggetto di configurazione che specifichi il generatore di SignalR.</span><span class="sxs-lookup"><span data-stu-id="2f3ad-158">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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
