---
title: Integrare ASP.NET Core componenti Razor in app Razor Pages e MVC
author: guardrex
description: Informazioni sugli scenari di data binding per i componenti e gli elementi DOM nelle app Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/integrate-components
ms.openlocfilehash: de1a37ffd9456c956e3d84fcc69431ecb794513c
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663317"
---
# <a name="integrate-aspnet-core-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="a52dc-103">Integrare ASP.NET Core componenti Razor in app Razor Pages e MVC</span><span class="sxs-lookup"><span data-stu-id="a52dc-103">Integrate ASP.NET Core Razor components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="a52dc-104">Di [Luke Latham](https://github.com/guardrex) e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="a52dc-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="a52dc-105">I componenti Razor possono essere integrati nelle app Razor Pages e MVC.</span><span class="sxs-lookup"><span data-stu-id="a52dc-105">Razor components can be integrated into Razor Pages and MVC apps.</span></span> <span data-ttu-id="a52dc-106">Quando viene eseguito il rendering della pagina o della vista, è possibile eseguire il rendering dei componenti contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="a52dc-106">When the page or view is rendered, components can be prerendered at the same time.</span></span>

## <a name="prepare-the-app-to-use-components-in-pages-and-views"></a><span data-ttu-id="a52dc-107">Preparare l'app per l'uso di componenti in pagine e visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="a52dc-107">Prepare the app to use components in pages and views</span></span>

<span data-ttu-id="a52dc-108">Un'app Razor Pages o MVC esistente può integrare i componenti Razor in pagine e visualizzazioni:</span><span class="sxs-lookup"><span data-stu-id="a52dc-108">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="a52dc-109">Nel file di layout dell'app ( *_Layout. cshtml*):</span><span class="sxs-lookup"><span data-stu-id="a52dc-109">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="a52dc-110">Aggiungere il tag di `<base>` seguente all'elemento `<head>`:</span><span class="sxs-lookup"><span data-stu-id="a52dc-110">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="a52dc-111">Il valore `href` (il *percorso di base dell'app*) nell'esempio precedente presuppone che l'app si trovi nel percorso URL radice (`/`).</span><span class="sxs-lookup"><span data-stu-id="a52dc-111">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="a52dc-112">Se l'app è un'applicazione secondaria, seguire le istruzioni nella sezione *percorso di base dell'app* dell'articolo <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="a52dc-112">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="a52dc-113">Il file *_Layout. cshtml* si trova nella cartella *pages/Shared* in un'app Razor Pages o in una cartella *Views/Shared* in un'app MVC.</span><span class="sxs-lookup"><span data-stu-id="a52dc-113">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="a52dc-114">Aggiungere un tag `<script>` per lo script *blazer. Server. js* immediatamente prima del tag di chiusura `</body>`:</span><span class="sxs-lookup"><span data-stu-id="a52dc-114">Add a `<script>` tag for the *blazor.server.js* script immediately before of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="a52dc-115">Il Framework aggiunge lo script *blazer. Server. js* all'app.</span><span class="sxs-lookup"><span data-stu-id="a52dc-115">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="a52dc-116">Non è necessario aggiungere manualmente lo script all'app.</span><span class="sxs-lookup"><span data-stu-id="a52dc-116">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="a52dc-117">Aggiungere un file *_Imports. Razor* alla cartella radice del progetto con il contenuto seguente (modificare l'ultimo spazio dei nomi, `MyAppNamespace`, nello spazio dei nomi dell'app):</span><span class="sxs-lookup"><span data-stu-id="a52dc-117">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

   ```razor
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. <span data-ttu-id="a52dc-118">In `Startup.ConfigureServices`registrare il servizio Server Blazer:</span><span class="sxs-lookup"><span data-stu-id="a52dc-118">In `Startup.ConfigureServices`, register the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="a52dc-119">In `Startup.Configure`aggiungere l'endpoint dell'hub blazer per `app.UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="a52dc-119">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="a52dc-120">Integrare i componenti in qualsiasi pagina o visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="a52dc-120">Integrate components into any page or view.</span></span> <span data-ttu-id="a52dc-121">Per ulteriori informazioni, vedere la sezione [rendering dei componenti da una pagina o vista](#render-components-from-a-page-or-view) .</span><span class="sxs-lookup"><span data-stu-id="a52dc-121">For more information, see the [Render components from a page or view](#render-components-from-a-page-or-view) section.</span></span>

## <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="a52dc-122">Usare componenti instradabili in un'app Razor Pages</span><span class="sxs-lookup"><span data-stu-id="a52dc-122">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="a52dc-123">*Questa sezione riguarda l'aggiunta di componenti che sono direttamente instradabili dalle richieste degli utenti.*</span><span class="sxs-lookup"><span data-stu-id="a52dc-123">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="a52dc-124">Per supportare componenti Razor instradabili nelle app Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="a52dc-124">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="a52dc-125">Seguire le istruzioni riportate nella sezione [preparare l'app per l'uso dei componenti in pagine e viste](#prepare-the-app-to-use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="a52dc-125">Follow the guidance in the [Prepare the app to use components in pages and views](#prepare-the-app-to-use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="a52dc-126">Aggiungere un file *app. Razor* alla radice del progetto con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="a52dc-126">Add an *App.razor* file to the project root with the following content:</span></span>

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

1. <span data-ttu-id="a52dc-127">Aggiungere un file *_Host. cshtml* alla cartella *pages* con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="a52dc-127">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="a52dc-128">I componenti usano il file Shared *_Layout. cshtml* per il layout.</span><span class="sxs-lookup"><span data-stu-id="a52dc-128">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="a52dc-129">Aggiungere una route con priorità bassa per la pagina *_Host. cshtml* alla configurazione dell'endpoint in `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="a52dc-129">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="a52dc-130">Aggiungere componenti instradabili all'app.</span><span class="sxs-lookup"><span data-stu-id="a52dc-130">Add routable components to the app.</span></span> <span data-ttu-id="a52dc-131">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a52dc-131">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="a52dc-132">Per ulteriori informazioni sugli spazi dei nomi, vedere la sezione relativa agli [spazi dei nomi dei componenti](#component-namespaces) .</span><span class="sxs-lookup"><span data-stu-id="a52dc-132">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="a52dc-133">Usare componenti instradabili in un'app MVC</span><span class="sxs-lookup"><span data-stu-id="a52dc-133">Use routable components in an MVC app</span></span>

<span data-ttu-id="a52dc-134">*Questa sezione riguarda l'aggiunta di componenti che sono direttamente instradabili dalle richieste degli utenti.*</span><span class="sxs-lookup"><span data-stu-id="a52dc-134">*This section pertains to adding components that are directly routable from user requests.*</span></span>

<span data-ttu-id="a52dc-135">Per supportare i componenti Razor instradabili nelle app MVC:</span><span class="sxs-lookup"><span data-stu-id="a52dc-135">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="a52dc-136">Seguire le istruzioni riportate nella sezione [preparare l'app per l'uso dei componenti in pagine e viste](#prepare-the-app-to-use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="a52dc-136">Follow the guidance in the [Prepare the app to use components in pages and views](#prepare-the-app-to-use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="a52dc-137">Aggiungere un file *app. Razor* alla radice del progetto con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="a52dc-137">Add an *App.razor* file to the root of the project with the following content:</span></span>

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

1. <span data-ttu-id="a52dc-138">Aggiungere un file *_Host. cshtml* alla cartella *views/Home* con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="a52dc-138">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="a52dc-139">I componenti usano il file Shared *_Layout. cshtml* per il layout.</span><span class="sxs-lookup"><span data-stu-id="a52dc-139">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="a52dc-140">Aggiungere un'azione al controller Home:</span><span class="sxs-lookup"><span data-stu-id="a52dc-140">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="a52dc-141">Aggiungere una route con priorità bassa per l'azione del controller che restituisce la vista *_Host. cshtml* alla configurazione dell'endpoint in `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="a52dc-141">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="a52dc-142">Creare una cartella *pages* e aggiungere componenti instradabili all'app.</span><span class="sxs-lookup"><span data-stu-id="a52dc-142">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="a52dc-143">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a52dc-143">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="a52dc-144">Per ulteriori informazioni sugli spazi dei nomi, vedere la sezione relativa agli [spazi dei nomi dei componenti](#component-namespaces) .</span><span class="sxs-lookup"><span data-stu-id="a52dc-144">For more information on namespaces, see the [Component namespaces](#component-namespaces) section.</span></span>

## <a name="component-namespaces"></a><span data-ttu-id="a52dc-145">Spazi dei nomi dei componenti</span><span class="sxs-lookup"><span data-stu-id="a52dc-145">Component namespaces</span></span>

<span data-ttu-id="a52dc-146">Quando si usa una cartella personalizzata per archiviare i componenti dell'app, aggiungere lo spazio dei nomi che rappresenta la cartella alla pagina/visualizzazione o al file *_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="a52dc-146">When using a custom folder to hold the app's components, add the namespace representing the folder to either the page/view or to the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="a52dc-147">Nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="a52dc-147">In the following example:</span></span>

* <span data-ttu-id="a52dc-148">Modificare `MyAppNamespace` nello spazio dei nomi dell'app.</span><span class="sxs-lookup"><span data-stu-id="a52dc-148">Change `MyAppNamespace` to the app's namespace.</span></span>
* <span data-ttu-id="a52dc-149">Se una cartella denominata *Components* non viene utilizzata per conservare i componenti, modificare `Components` alla cartella in cui risiedono i componenti.</span><span class="sxs-lookup"><span data-stu-id="a52dc-149">If a folder named *Components* isn't used to hold the components, change `Components` to the folder where the components reside.</span></span>

```cshtml
@using MyAppNamespace.Components
```

<span data-ttu-id="a52dc-150">Il file *_ViewImports. cshtml* si trova nella cartella *pagine* di un'app Razor Pages o nella cartella *views* di un'app MVC.</span><span class="sxs-lookup"><span data-stu-id="a52dc-150">The *_ViewImports.cshtml* file is located in the *Pages* folder of a Razor Pages app or the *Views* folder of an MVC app.</span></span>

<span data-ttu-id="a52dc-151">Per altre informazioni, vedere <xref:blazor/components#import-components>.</span><span class="sxs-lookup"><span data-stu-id="a52dc-151">For more information, see <xref:blazor/components#import-components>.</span></span>

## <a name="render-components-from-a-page-or-view"></a><span data-ttu-id="a52dc-152">Eseguire il rendering dei componenti da una pagina o da una vista</span><span class="sxs-lookup"><span data-stu-id="a52dc-152">Render components from a page or view</span></span>

<span data-ttu-id="a52dc-153">*Questa sezione riguarda l'aggiunta di componenti a pagine o viste, in cui i componenti non sono direttamente instradabili dalle richieste degli utenti.*</span><span class="sxs-lookup"><span data-stu-id="a52dc-153">*This section pertains to adding components to pages or views, where the components aren't directly routable from user requests.*</span></span>

<span data-ttu-id="a52dc-154">Per eseguire il rendering di un componente da una pagina o da una vista, usare l'helper Tag `Component`:</span><span class="sxs-lookup"><span data-stu-id="a52dc-154">To render a component from a page or view, use the `Component` Tag Helper:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="a52dc-155">Il tipo di parametro deve essere serializzabile JSON, che in genere significa che il tipo deve avere un costruttore predefinito e le proprietà impostabili.</span><span class="sxs-lookup"><span data-stu-id="a52dc-155">The parameter type must be JSON serializable, which typically means that the type must have a default constructor and settable properties.</span></span> <span data-ttu-id="a52dc-156">Ad esempio, è possibile specificare un valore per `IncrementAmount` perché il tipo di `IncrementAmount` è un `int`, ovvero un tipo primitivo supportato dal serializzatore JSON.</span><span class="sxs-lookup"><span data-stu-id="a52dc-156">For example, you can specify a value for `IncrementAmount` because the type of `IncrementAmount` is an `int`, which is a primitive type supported by the JSON serializer.</span></span>

<span data-ttu-id="a52dc-157">`RenderMode` configura se il componente:</span><span class="sxs-lookup"><span data-stu-id="a52dc-157">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="a52dc-158">Viene preeseguito nella pagina.</span><span class="sxs-lookup"><span data-stu-id="a52dc-158">Is prerendered into the page.</span></span>
* <span data-ttu-id="a52dc-159">Viene visualizzato come HTML statico nella pagina o se include le informazioni necessarie per eseguire il bootstrap di un'app Blazor dall'agente utente.</span><span class="sxs-lookup"><span data-stu-id="a52dc-159">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="a52dc-160">Descrizione</span><span class="sxs-lookup"><span data-stu-id="a52dc-160">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="a52dc-161">Esegue il rendering del componente in HTML statico e include un marcatore per un'app Server Blazor.</span><span class="sxs-lookup"><span data-stu-id="a52dc-161">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="a52dc-162">Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="a52dc-162">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="a52dc-163">Esegue il rendering di un marcatore per un'app Server Blazor.</span><span class="sxs-lookup"><span data-stu-id="a52dc-163">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="a52dc-164">L'output del componente non è incluso.</span><span class="sxs-lookup"><span data-stu-id="a52dc-164">Output from the component isn't included.</span></span> <span data-ttu-id="a52dc-165">Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="a52dc-165">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="a52dc-166">Esegue il rendering del componente in HTML statico.</span><span class="sxs-lookup"><span data-stu-id="a52dc-166">Renders the component into static HTML.</span></span> |

<span data-ttu-id="a52dc-167">Mentre le pagine e le visualizzazioni possono usare i componenti, il contrario non è vero.</span><span class="sxs-lookup"><span data-stu-id="a52dc-167">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="a52dc-168">I componenti non possono usare scenari di visualizzazione e di pagina specifici, ad esempio visualizzazioni parziali e sezioni.</span><span class="sxs-lookup"><span data-stu-id="a52dc-168">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="a52dc-169">Per usare la logica dalla visualizzazione parziale in un componente, scomporre la logica di visualizzazione parziale in un componente.</span><span class="sxs-lookup"><span data-stu-id="a52dc-169">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="a52dc-170">Il rendering dei componenti server da una pagina HTML statica non è supportato.</span><span class="sxs-lookup"><span data-stu-id="a52dc-170">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="a52dc-171">Per ulteriori informazioni sulla modalità di rendering dei componenti, sullo stato del componente e sull'helper tag di `Component`, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="a52dc-171">For more information on how components are rendered, component state, and the `Component` Tag Helper, see the following articles:</span></span>

* <xref:blazor/hosting-models>
* <xref:blazor/hosting-model-configuration>
