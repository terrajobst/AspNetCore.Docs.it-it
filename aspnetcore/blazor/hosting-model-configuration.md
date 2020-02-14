---
title: ASP.NET Core Blazor configurazione del modello di hosting
author: guardrex
description: Informazioni su Blazor la configurazione del modello di hosting, inclusa la procedura per integrare i componenti Razor in app Razor Pages e MVC.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/04/2020
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-model-configuration
ms.openlocfilehash: 91dd1aa32bb5165845eb4794377a50ccbd01adc3
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2020
ms.locfileid: "77215060"
---
# <a name="aspnet-core-blazor-hosting-model-configuration"></a><span data-ttu-id="9d706-103">Configurazione del modello di hosting di ASP.NET Core Blazer</span><span class="sxs-lookup"><span data-stu-id="9d706-103">ASP.NET Core Blazor hosting model configuration</span></span>

<span data-ttu-id="9d706-104">Di [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="9d706-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="9d706-105">Questo articolo illustra la configurazione del modello di hosting.</span><span class="sxs-lookup"><span data-stu-id="9d706-105">This article covers hosting model configuration.</span></span>

<!-- For future use:

## Blazor WebAssembly

-->

## <a name="blazor-server"></a><span data-ttu-id="9d706-106">Server Blazor</span><span class="sxs-lookup"><span data-stu-id="9d706-106">Blazor Server</span></span>

### <a name="integrate-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="9d706-107">Integrare i componenti Razor in app Razor Pages e MVC</span><span class="sxs-lookup"><span data-stu-id="9d706-107">Integrate Razor components into Razor Pages and MVC apps</span></span>

#### <a name="use-components-in-pages-and-views"></a><span data-ttu-id="9d706-108">Usare i componenti in pagine e visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="9d706-108">Use components in pages and views</span></span>

<span data-ttu-id="9d706-109">Un'app Razor Pages o MVC esistente può integrare i componenti Razor in pagine e visualizzazioni:</span><span class="sxs-lookup"><span data-stu-id="9d706-109">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="9d706-110">Nel file di layout dell'app ( *_Layout. cshtml*):</span><span class="sxs-lookup"><span data-stu-id="9d706-110">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="9d706-111">Aggiungere il tag di `<base>` seguente all'elemento `<head>`:</span><span class="sxs-lookup"><span data-stu-id="9d706-111">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="9d706-112">Il valore `href` (il *percorso di base dell'app*) nell'esempio precedente presuppone che l'app si trovi nel percorso URL radice (`/`).</span><span class="sxs-lookup"><span data-stu-id="9d706-112">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="9d706-113">Se l'app è un'applicazione secondaria, seguire le istruzioni nella sezione *percorso di base dell'app* dell'articolo <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="9d706-113">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="9d706-114">Il file *_Layout. cshtml* si trova nella cartella *pages/Shared* in un'app Razor Pages o in una cartella *Views/Shared* in un'app MVC.</span><span class="sxs-lookup"><span data-stu-id="9d706-114">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="9d706-115">Aggiungere un tag `<script>` per lo script *blazer. Server. js* immediatamente prima del tag di chiusura `</body>`:</span><span class="sxs-lookup"><span data-stu-id="9d706-115">Add a `<script>` tag for the *blazor.server.js* script right before of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="9d706-116">Il Framework aggiunge lo script *blazer. Server. js* all'app.</span><span class="sxs-lookup"><span data-stu-id="9d706-116">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="9d706-117">Non è necessario aggiungere manualmente lo script all'app.</span><span class="sxs-lookup"><span data-stu-id="9d706-117">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="9d706-118">Aggiungere un file *_Imports. Razor* alla cartella radice del progetto con il contenuto seguente (modificare l'ultimo spazio dei nomi, `MyAppNamespace`, nello spazio dei nomi dell'app):</span><span class="sxs-lookup"><span data-stu-id="9d706-118">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

   ```csharp
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. <span data-ttu-id="9d706-119">In `Startup.ConfigureServices`registrare il servizio Server Blazer:</span><span class="sxs-lookup"><span data-stu-id="9d706-119">In `Startup.ConfigureServices`, register the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="9d706-120">In `Startup.Configure`aggiungere l'endpoint dell'hub blazer per `app.UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="9d706-120">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="9d706-121">Integrare i componenti in qualsiasi pagina o visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="9d706-121">Integrate components into any page or view.</span></span> <span data-ttu-id="9d706-122">Per ulteriori informazioni, vedere la sezione *integrare componenti in Razor Pages e app MVC* dell'articolo <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="9d706-122">For more information, see the *Integrate components into Razor Pages and MVC apps* section of the <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps> article.</span></span>

#### <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="9d706-123">Usare componenti instradabili in un'app Razor Pages</span><span class="sxs-lookup"><span data-stu-id="9d706-123">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="9d706-124">Per supportare componenti Razor instradabili nelle app Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="9d706-124">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="9d706-125">Seguire le istruzioni riportate nella sezione [usare i componenti in pagine e viste](#use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="9d706-125">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="9d706-126">Aggiungere un file *app. Razor* alla radice del progetto con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="9d706-126">Add an *App.razor* file to the root of the project with the following content:</span></span>

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. <span data-ttu-id="9d706-127">Aggiungere un file *_Host. cshtml* alla cartella *pages* con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="9d706-127">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="9d706-128">I componenti usano il file Shared *_Layout. cshtml* per il layout.</span><span class="sxs-lookup"><span data-stu-id="9d706-128">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="9d706-129">Aggiungere una route con priorità bassa per la pagina *_Host. cshtml* alla configurazione dell'endpoint in `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="9d706-129">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="9d706-130">Aggiungere componenti instradabili all'app.</span><span class="sxs-lookup"><span data-stu-id="9d706-130">Add routable components to the app.</span></span> <span data-ttu-id="9d706-131">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9d706-131">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="9d706-132">Quando si usa una cartella personalizzata per archiviare i componenti dell'app, aggiungere lo spazio dei nomi che rappresenta la cartella al file *pages/_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="9d706-132">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Pages/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="9d706-133">Per altre informazioni, vedere <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="9d706-133">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

#### <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="9d706-134">Usare componenti instradabili in un'app MVC</span><span class="sxs-lookup"><span data-stu-id="9d706-134">Use routable components in an MVC app</span></span>

<span data-ttu-id="9d706-135">Per supportare i componenti Razor instradabili nelle app MVC:</span><span class="sxs-lookup"><span data-stu-id="9d706-135">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="9d706-136">Seguire le istruzioni riportate nella sezione [usare i componenti in pagine e viste](#use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="9d706-136">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="9d706-137">Aggiungere un file *app. Razor* alla radice del progetto con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="9d706-137">Add an *App.razor* file to the root of the project with the following content:</span></span>

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. <span data-ttu-id="9d706-138">Aggiungere un file *_Host. cshtml* alla cartella *views/Home* con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="9d706-138">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="9d706-139">I componenti usano il file Shared *_Layout. cshtml* per il layout.</span><span class="sxs-lookup"><span data-stu-id="9d706-139">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="9d706-140">Aggiungere un'azione al controller Home:</span><span class="sxs-lookup"><span data-stu-id="9d706-140">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="9d706-141">Aggiungere una route con priorità bassa per l'azione del controller che restituisce la vista *_Host. cshtml* alla configurazione dell'endpoint in `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="9d706-141">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="9d706-142">Creare una cartella *pages* e aggiungere componenti instradabili all'app.</span><span class="sxs-lookup"><span data-stu-id="9d706-142">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="9d706-143">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="9d706-143">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="9d706-144">Quando si usa una cartella personalizzata per archiviare i componenti dell'app, aggiungere lo spazio dei nomi che rappresenta la cartella al file *views/_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="9d706-144">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="9d706-145">Per altre informazioni, vedere <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="9d706-145">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="9d706-146">Riflette lo stato di connessione nell'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="9d706-146">Reflect the connection state in the UI</span></span>

<span data-ttu-id="9d706-147">Quando il client rileva che la connessione è stata persa, viene visualizzata un'interfaccia utente predefinita quando il client tenta di riconnettersi.</span><span class="sxs-lookup"><span data-stu-id="9d706-147">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="9d706-148">Se la riconnessione non riesce, all'utente viene offerta l'opzione per riprovare.</span><span class="sxs-lookup"><span data-stu-id="9d706-148">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="9d706-149">Per personalizzare l'interfaccia utente, definire un elemento con un `id` di `components-reconnect-modal` nel `<body>` della pagina Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="9d706-149">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="9d706-150">Nella tabella seguente vengono descritte le classi CSS applicate all'elemento `components-reconnect-modal`.</span><span class="sxs-lookup"><span data-stu-id="9d706-150">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="9d706-151">Classe CSS</span><span class="sxs-lookup"><span data-stu-id="9d706-151">CSS class</span></span>                       | <span data-ttu-id="9d706-152">Indica&hellip;</span><span class="sxs-lookup"><span data-stu-id="9d706-152">Indicates&hellip;</span></span> |
| ------------------------------- | ----------------- |
| `components-reconnect-show`     | <span data-ttu-id="9d706-153">Connessione persa.</span><span class="sxs-lookup"><span data-stu-id="9d706-153">A lost connection.</span></span> <span data-ttu-id="9d706-154">È in corso un tentativo di riconnessione del client.</span><span class="sxs-lookup"><span data-stu-id="9d706-154">The client is attempting to reconnect.</span></span> <span data-ttu-id="9d706-155">Mostra il modale.</span><span class="sxs-lookup"><span data-stu-id="9d706-155">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="9d706-156">Viene ristabilita una connessione attiva al server.</span><span class="sxs-lookup"><span data-stu-id="9d706-156">An active connection is re-established to the server.</span></span> <span data-ttu-id="9d706-157">Nascondere il modale.</span><span class="sxs-lookup"><span data-stu-id="9d706-157">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="9d706-158">Riconnessione non riuscita. probabilmente a causa di un errore di rete.</span><span class="sxs-lookup"><span data-stu-id="9d706-158">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="9d706-159">Per tentare la riconnessione, chiamare `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="9d706-159">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="9d706-160">Riconnessione rifiutata.</span><span class="sxs-lookup"><span data-stu-id="9d706-160">Reconnection rejected.</span></span> <span data-ttu-id="9d706-161">Il server è stato raggiunto ma ha rifiutato la connessione e lo stato dell'utente nel server è andato perso.</span><span class="sxs-lookup"><span data-stu-id="9d706-161">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="9d706-162">Per ricaricare l'app, chiamare `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="9d706-162">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="9d706-163">Questo stato di connessione può verificarsi nei casi seguenti:</span><span class="sxs-lookup"><span data-stu-id="9d706-163">This connection state may result when:</span></span><ul><li><span data-ttu-id="9d706-164">Si verifica un arresto anomalo del circuito sul lato server.</span><span class="sxs-lookup"><span data-stu-id="9d706-164">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="9d706-165">Il client viene disconnesso abbastanza a lungo da consentire al server di eliminare lo stato dell'utente.</span><span class="sxs-lookup"><span data-stu-id="9d706-165">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="9d706-166">Le istanze dei componenti con cui l'utente interagisce sono state eliminate.</span><span class="sxs-lookup"><span data-stu-id="9d706-166">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="9d706-167">Il server viene riavviato oppure il processo di lavoro dell'app viene riciclato.</span><span class="sxs-lookup"><span data-stu-id="9d706-167">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="render-mode"></a><span data-ttu-id="9d706-168">Modalità di rendering</span><span class="sxs-lookup"><span data-stu-id="9d706-168">Render mode</span></span>

<span data-ttu-id="9d706-169">Per impostazione predefinita, le app del server Blazor sono configurate per eseguire il prerendering dell'interfaccia utente nel server prima che venga stabilita la connessione client al server.</span><span class="sxs-lookup"><span data-stu-id="9d706-169">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="9d706-170">Questa impostazione è configurata nella pagina Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="9d706-170">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="9d706-171">`RenderMode` configura se il componente:</span><span class="sxs-lookup"><span data-stu-id="9d706-171">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="9d706-172">Viene preeseguito nella pagina.</span><span class="sxs-lookup"><span data-stu-id="9d706-172">Is prerendered into the page.</span></span>
* <span data-ttu-id="9d706-173">Viene visualizzato come HTML statico nella pagina o se include le informazioni necessarie per eseguire il bootstrap di un'app Blazor dall'agente utente.</span><span class="sxs-lookup"><span data-stu-id="9d706-173">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="9d706-174">Descrizione</span><span class="sxs-lookup"><span data-stu-id="9d706-174">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="9d706-175">Esegue il rendering del componente in HTML statico e include un marcatore per un'app Server Blazor.</span><span class="sxs-lookup"><span data-stu-id="9d706-175">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="9d706-176">Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="9d706-176">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="9d706-177">Esegue il rendering di un marcatore per un'app Server Blazor.</span><span class="sxs-lookup"><span data-stu-id="9d706-177">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="9d706-178">L'output del componente non è incluso.</span><span class="sxs-lookup"><span data-stu-id="9d706-178">Output from the component isn't included.</span></span> <span data-ttu-id="9d706-179">Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="9d706-179">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="9d706-180">Esegue il rendering del componente in HTML statico.</span><span class="sxs-lookup"><span data-stu-id="9d706-180">Renders the component into static HTML.</span></span> |

<span data-ttu-id="9d706-181">Il rendering dei componenti server da una pagina HTML statica non è supportato.</span><span class="sxs-lookup"><span data-stu-id="9d706-181">Rendering server components from a static HTML page isn't supported.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="9d706-182">Eseguire il rendering di componenti interattivi con stato da pagine e visualizzazioni Razor</span><span class="sxs-lookup"><span data-stu-id="9d706-182">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="9d706-183">I componenti interattivi con stato possono essere aggiunti a una pagina o a una visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="9d706-183">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="9d706-184">Quando viene eseguito il rendering della pagina o della visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="9d706-184">When the page or view renders:</span></span>

* <span data-ttu-id="9d706-185">Il componente viene preeseguito con la pagina o la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="9d706-185">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="9d706-186">Lo stato iniziale del componente utilizzato per il prerendering viene perso.</span><span class="sxs-lookup"><span data-stu-id="9d706-186">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="9d706-187">Quando viene stabilita la connessione SignalR viene creato il nuovo stato del componente.</span><span class="sxs-lookup"><span data-stu-id="9d706-187">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="9d706-188">La pagina Razor seguente esegue il rendering di un componente `Counter`:</span><span class="sxs-lookup"><span data-stu-id="9d706-188">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="9d706-189">Eseguire il rendering di componenti non interattivi da pagine e visualizzazioni Razor</span><span class="sxs-lookup"><span data-stu-id="9d706-189">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="9d706-190">Nella pagina Razor seguente il componente `Counter` viene sottoposto a rendering statico con un valore iniziale specificato utilizzando un form:</span><span class="sxs-lookup"><span data-stu-id="9d706-190">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

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

<span data-ttu-id="9d706-191">Poiché `MyComponent` viene sottoposto a rendering statico, il componente non può essere interattivo.</span><span class="sxs-lookup"><span data-stu-id="9d706-191">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="9d706-192">Configurare il client di SignalR per le app Blazor server</span><span class="sxs-lookup"><span data-stu-id="9d706-192">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="9d706-193">In alcuni casi, è necessario configurare il client SignalR usato dalle app Blazor server.</span><span class="sxs-lookup"><span data-stu-id="9d706-193">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="9d706-194">È ad esempio possibile configurare la registrazione nel client di SignalR per diagnosticare un problema di connessione.</span><span class="sxs-lookup"><span data-stu-id="9d706-194">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="9d706-195">Per configurare il client di SignalR nel file *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="9d706-195">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="9d706-196">Aggiungere un attributo `autostart="false"` al tag `<script>` per lo script `blazor.server.js`.</span><span class="sxs-lookup"><span data-stu-id="9d706-196">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="9d706-197">Chiamare `Blazor.start` e passare un oggetto di configurazione che specifichi il generatore di SignalR.</span><span class="sxs-lookup"><span data-stu-id="9d706-197">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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
