---
title: Usare JavaScriptServices per creare applicazioni a pagina singola in ASP.NET Core
author: scottaddie
description: Scopri i vantaggi dell'uso di JavaScriptServices per creare un applicazione a pagina singola (SPA) supportati da ASP.NET Core.
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
uid: client-side/spa-services
ms.openlocfilehash: 6ac922d82e5c93343cd0e9df312719c6df121dcb
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37434000"
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a><span data-ttu-id="4a2c1-103">Usare JavaScriptServices per creare applicazioni a pagina singola in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4a2c1-103">Use JavaScriptServices to Create Single Page Applications in ASP.NET Core</span></span>

<span data-ttu-id="4a2c1-104">Dal [Scott Addie](https://github.com/scottaddie) e [Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="4a2c1-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="4a2c1-105">Un applicazione a pagina singola (SPA) è un tipo comune di applicazione web a causa l'esperienza utente avanzata inerente.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="4a2c1-106">L'integrazione Framework SPA o librerie lato client, ad esempio [Angular](https://angular.io/) oppure [React](https://facebook.github.io/react/), con i framework lato server, ad esempio ASP.NET Core può essere difficile.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="4a2c1-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) è stato sviluppato per ridurre gli attriti nel processo di integrazione.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="4a2c1-108">In questo modo l'operazione senza problemi tra i diversi client e gli stack di tecnologie di server.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-108">It enables seamless operation between the different client and server technology stacks.</span></span>

<span data-ttu-id="4a2c1-109">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4a2c1-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="4a2c1-110">Che cos'è JavaScriptServices?</span><span class="sxs-lookup"><span data-stu-id="4a2c1-110">What is JavaScriptServices?</span></span>

<span data-ttu-id="4a2c1-111">JavaScriptServices è una raccolta di tecnologie lato client per ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-111">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="4a2c1-112">L'obiettivo consiste nel posizionare ASP.NET Core come piattaforma del lato server per compilazione SPAs preferita degli sviluppatori.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-112">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="4a2c1-113">JavaScriptServices è costituito da tre pacchetti NuGet distinti:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-113">JavaScriptServices consists of three distinct NuGet packages:</span></span>
* <span data-ttu-id="4a2c1-114">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="4a2c1-114">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="4a2c1-115">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="4a2c1-115">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="4a2c1-116">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="4a2c1-116">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="4a2c1-117">Questi pacchetti sono utili se è:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-117">These packages are useful if you:</span></span>
* <span data-ttu-id="4a2c1-118">Eseguire JavaScript nel server</span><span class="sxs-lookup"><span data-stu-id="4a2c1-118">Run JavaScript on the server</span></span>
* <span data-ttu-id="4a2c1-119">Usare un framework SPA o una raccolta</span><span class="sxs-lookup"><span data-stu-id="4a2c1-119">Use a SPA framework or library</span></span>
* <span data-ttu-id="4a2c1-120">Creare gli asset client-side con Webpack</span><span class="sxs-lookup"><span data-stu-id="4a2c1-120">Build client-side assets with Webpack</span></span>

<span data-ttu-id="4a2c1-121">Gran parte dello stato attivo in questo articolo viene posizionata sull'uso del pacchetto SpaServices.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-121">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="4a2c1-122">Che cos'è SpaServices?</span><span class="sxs-lookup"><span data-stu-id="4a2c1-122">What is SpaServices?</span></span>

<span data-ttu-id="4a2c1-123">SpaServices è stato creato per posizionare ASP.NET Core come piattaforma del lato server per compilazione SPAs preferita degli sviluppatori.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-123">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="4a2c1-124">Per sviluppare SPAs con ASP.NET Core non è necessario SpaServices e non venga bloccato è un framework client specifico.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-124">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="4a2c1-125">SpaServices fornisce utili dell'infrastruttura, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-125">SpaServices provides useful infrastructure such as:</span></span>
* [<span data-ttu-id="4a2c1-126">Rendering lato server preliminare</span><span class="sxs-lookup"><span data-stu-id="4a2c1-126">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="4a2c1-127">Dev Middleware Webpack</span><span class="sxs-lookup"><span data-stu-id="4a2c1-127">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="4a2c1-128">Sostituzione di un modulo di accesso frequente</span><span class="sxs-lookup"><span data-stu-id="4a2c1-128">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="4a2c1-129">Helper di routing</span><span class="sxs-lookup"><span data-stu-id="4a2c1-129">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="4a2c1-130">Complessivamente, questi componenti di infrastruttura migliorano sia il flusso di lavoro di sviluppo e l'esperienza di runtime.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-130">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="4a2c1-131">I componenti possono essere adottati singolarmente.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-131">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="4a2c1-132">Prerequisiti per l'uso SpaServices</span><span class="sxs-lookup"><span data-stu-id="4a2c1-132">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="4a2c1-133">Per usare SpaServices, installare quanto segue:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-133">To work with SpaServices, install the following:</span></span>
* <span data-ttu-id="4a2c1-134">[Node. js](https://nodejs.org/) (versione 6 o versione successiva) con npm</span><span class="sxs-lookup"><span data-stu-id="4a2c1-134">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
  * <span data-ttu-id="4a2c1-135">Per verificare questi componenti vengono installati e sono disponibili, eseguire il comando seguente dalla riga di comando:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-135">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="4a2c1-136">Nota: Se si distribuisce in un sito web di Azure, non devi eseguire alcuna operazione qui &mdash; Node. js sia installato e disponibile negli ambienti server.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-136">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * <span data-ttu-id="4a2c1-137">Se si usa Windows usando Visual Studio 2017, il SDK è installato, selezionando il **sviluppo multipiattaforma .NET Core** carico di lavoro.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-137">If you're on Windows using Visual Studio 2017, the SDK is installed by selecting the **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="4a2c1-138">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) pacchetto NuGet</span><span class="sxs-lookup"><span data-stu-id="4a2c1-138">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="4a2c1-139">Rendering lato server preliminare</span><span class="sxs-lookup"><span data-stu-id="4a2c1-139">Server-side prerendering</span></span>

<span data-ttu-id="4a2c1-140">Un'applicazione (noto anche come siano isomorfi) universale è un'applicazione JavaScript in grado di eseguire sia nel server e client.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-140">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="4a2c1-141">Angular, React e altri framework comuni fornire una piattaforma universale per questo stile di sviluppo dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-141">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="4a2c1-142">L'idea è prima di tutto eseguire il rendering i componenti del framework sul server tramite Node. js e delegare quindi ulteriormente l'esecuzione nel client.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-142">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="4a2c1-143">ASP.NET Core [helper Tag](xref:mvc/views/tag-helpers/intro) fornita da SpaServices semplificano l'implementazione del rendering lato server preliminare chiamando le funzioni di JavaScript nel server.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-143">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="4a2c1-144">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="4a2c1-144">Prerequisites</span></span>

<span data-ttu-id="4a2c1-145">Installare gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-145">Install the following:</span></span>
* <span data-ttu-id="4a2c1-146">[il rendering ASPNET-preliminare](https://www.npmjs.com/package/aspnet-prerendering) pacchetto npm:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-146">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="4a2c1-147">Configurazione</span><span class="sxs-lookup"><span data-stu-id="4a2c1-147">Configuration</span></span>

<span data-ttu-id="4a2c1-148">Gli helper Tag resi individuabili tramite la registrazione dello spazio dei nomi del progetto *viewimports. cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-148">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="4a2c1-149">Questi helper Tag estraggono le complessità di comunicare direttamente con le API di basso livello tramite una sintassi simile a HTML all'interno della visualizzazione Razor:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-149">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="4a2c1-150">Il `asp-prerender-module` Helper Tag</span><span class="sxs-lookup"><span data-stu-id="4a2c1-150">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="4a2c1-151">Il `asp-prerender-module` Helper Tag, usati nell'esempio di codice precedente, viene eseguito *ClientApp/dist/main-server.js* sul server tramite Node. js.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-151">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="4a2c1-152">Per chiarezza, *main-server. js* file è un elemento dell'attività volta TypeScript e JavaScript il [Webpack](http://webpack.github.io/) processo di compilazione.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-152">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="4a2c1-153">Webpack definisce un alias di punto di ingresso del `main-server`; e, attraversamento del grafico delle dipendenze per questo alias inizia in corrispondenza di *ClientApp/avvio-server.ts* file:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-153">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="4a2c1-154">Nell'esempio seguente Angular, il *ClientApp/avvio-server.ts* file utilizza il `createServerRenderer` (funzione) e `RenderResult` digitare del `aspnet-prerendering` pacchetto npm per configurare il rendering di server tramite Node. js.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-154">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="4a2c1-155">Il markup HTML destinato per il rendering lato server viene passato a una chiamata di funzione di risoluzione, che viene eseguito il wrapping in un JavaScript fortemente tipizzato `Promise` oggetto.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-155">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="4a2c1-156">Il `Promise` significato dell'oggetto è in modo asincrono che fornisce il markup HTML per la pagina per l'inserimento nell'elemento segnaposto del DOM.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-156">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="4a2c1-157">Il `asp-prerender-data` Helper Tag</span><span class="sxs-lookup"><span data-stu-id="4a2c1-157">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="4a2c1-158">Se usato in combinazione con il `asp-prerender-module` Helper Tag, il `asp-prerender-data` Helper Tag può essere utilizzato per passare informazioni contestuali dalla vista Razor per JavaScript lato server.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-158">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="4a2c1-159">Ad esempio, il markup seguente passa i dati utente per il `main-server` modulo:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-159">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="4a2c1-160">I dati ricevuti `UserName` argomenti viene serializzato utilizzando il serializzatore JSON predefinito e viene archiviato nel `params.data` oggetto.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-160">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="4a2c1-161">Nell'esempio seguente Angular, i dati vengono utilizzati per costruire un messaggio di saluto personalizzato all'interno di un `h1` elemento:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-161">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="4a2c1-162">Nota: I nomi delle proprietà passati gli helper Tag sono rappresentati con **PascalCase** notation.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-162">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="4a2c1-163">Ciò contrasta in JavaScript, in cui gli stessi nomi di proprietà sono rappresentati con **camelCase**.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-163">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="4a2c1-164">La configurazione di serializzazione JSON di predefinita è responsabile di questa differenza.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-164">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="4a2c1-165">Per espandere l'esempio di codice precedente, i dati possono essere passati dal server per la visualizzazione da idratare le il `globals` proprietà fornite per il `resolve` funzione:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-165">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="4a2c1-166">Il `postList` matrice definita all'interno di `globals` oggetto è connesso al browser globale `window` oggetto.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-166">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="4a2c1-167">Questa variabile sottraendo in ambito globale consente di eliminare la duplicazione del lavoro richiesto, in particolare per quanto riguarda il caricamento dei dati stessi una sola volta nel server e nuovamente sul client.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-167">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![variabile globale postList collegato all'oggetto finestra](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="4a2c1-169">Dev Middleware Webpack</span><span class="sxs-lookup"><span data-stu-id="4a2c1-169">Webpack Dev Middleware</span></span>

<span data-ttu-id="4a2c1-170">[Dev Middleware Webpack](https://webpack.github.io/docs/webpack-dev-middleware.html) introduce un flusso di lavoro di sviluppo semplificato in base al quale Webpack compila risorse su richiesta.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-170">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="4a2c1-171">Il middleware viene compilato automaticamente e gestisce le risorse lato client quando una pagina viene ricaricata nel browser.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-171">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="4a2c1-172">L'approccio alternativo consiste nel richiamare manualmente Webpack tramite script di compilazione del progetto npm quando viene modificata una dipendenza di terze parti o il codice personalizzato.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-172">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="4a2c1-173">Script di compilazione un npm *package. JSON* file è illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-173">An npm build script in the *package.json* file is shown in the following example:</span></span>

[!code-json[](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a><span data-ttu-id="4a2c1-174">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="4a2c1-174">Prerequisites</span></span>

<span data-ttu-id="4a2c1-175">Installare gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-175">Install the following:</span></span>
* <span data-ttu-id="4a2c1-176">[ASPNET-webpack](https://www.npmjs.com/package/aspnet-webpack) pacchetto npm:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-176">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="4a2c1-177">Configurazione</span><span class="sxs-lookup"><span data-stu-id="4a2c1-177">Configuration</span></span>

<span data-ttu-id="4a2c1-178">Webpack Dev Middleware è registrato nella pipeline delle richieste HTTP tramite il codice seguente nel *Startup.cs* del file `Configure` metodo:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-178">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

<span data-ttu-id="4a2c1-179">Il `UseWebpackDevMiddleware` metodo di estensione deve essere chiamato prima [la registrazione dei file statici che ospita](xref:fundamentals/static-files) tramite il `UseStaticFiles` metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-179">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="4a2c1-180">Per motivi di sicurezza, registrare il middleware solo quando l'app viene eseguita in modalità di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-180">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="4a2c1-181">Il *webpack.config.js* del file `output.publicPath` proprietà indica al middleware di guardare il `dist` cartella per le modifiche:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-181">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="4a2c1-182">Sostituzione di un modulo di accesso frequente</span><span class="sxs-lookup"><span data-stu-id="4a2c1-182">Hot Module Replacement</span></span>

<span data-ttu-id="4a2c1-183">Può essere considerata di Webpack [modulo di sostituzione a caldo](https://webpack.js.org/concepts/hot-module-replacement/) funzionalità (HMR) come un'evoluzione del [Webpack Dev Middleware](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="4a2c1-183">Think of Webpack's [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="4a2c1-184">HMR introduce gli stessi vantaggi, ma semplifica ulteriormente il flusso di lavoro di sviluppo aggiornando automaticamente il contenuto della pagina dopo aver compilato le modifiche.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-184">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="4a2c1-185">Non confonderlo con l'aggiornamento del browser, che potrebbe interferire con la stato in memoria corrente e la sessione di debug di SPA.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-185">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="4a2c1-186">È presente un collegamento in tempo reale tra il servizio di Dev Middleware Webpack e il browser, ovvero vengono effettuato il push delle modifiche nel browser.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-186">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="4a2c1-187">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="4a2c1-187">Prerequisites</span></span>

<span data-ttu-id="4a2c1-188">Installare gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-188">Install the following:</span></span>
* <span data-ttu-id="4a2c1-189">[middleware hot webpack](https://www.npmjs.com/package/webpack-hot-middleware) pacchetto npm:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-189">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="4a2c1-190">Configurazione</span><span class="sxs-lookup"><span data-stu-id="4a2c1-190">Configuration</span></span>

<span data-ttu-id="4a2c1-191">Il componente HMR deve essere registrato nella pipeline di richieste HTTP di MVC nel `Configure` metodo:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-191">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="4a2c1-192">Come era true con [Webpack Dev Middleware](#webpack-dev-middleware), il `UseWebpackDevMiddleware` metodo di estensione deve essere chiamato prima di `UseStaticFiles` metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-192">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="4a2c1-193">Per motivi di sicurezza, registrare il middleware solo quando l'app viene eseguita in modalità di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-193">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="4a2c1-194">Il *webpack.config.js* file necessario definire un `plugins` matrice, anche se viene lasciata vuota:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-194">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="4a2c1-195">Dopo aver caricato l'app nel browser, scheda Console degli strumenti di sviluppo offre conferma dell'attivazione HMR:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-195">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![Messaggio di connessione di sostituzione di un modulo a caldo](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="4a2c1-197">Helper di routing</span><span class="sxs-lookup"><span data-stu-id="4a2c1-197">Routing helpers</span></span>

<span data-ttu-id="4a2c1-198">Nella maggior parte dei SPAs basate su ASP.NET Core, è opportuno routing sul lato client, oltre al routing lato server.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-198">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="4a2c1-199">I sistemi di routing SPA e MVC possono lavorare in modo indipendente senza interferenze.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-199">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="4a2c1-200">È presente, tuttavia, sono le problematiche ponendo un di case edge: identificazione delle risposte HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-200">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="4a2c1-201">Si consideri lo scenario in cui una route senza estensione di `/some/page` viene usato.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-201">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="4a2c1-202">Si supponga che la richiesta non modello-match una route sul lato server, ma il criterio di ricerca corrisponde a una route lato client.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-202">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="4a2c1-203">Ora si consideri una richiesta in ingresso per `/images/user-512.png`, che in genere prevede di trovare un file di immagine nel server.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-203">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="4a2c1-204">Se tale percorso risorsa richiesta non corrisponde a qualsiasi route sul lato server o dei file statici, è improbabile che l'applicazione lato client sarebbe di gestirlo, ovvero in genere si vuole restituire un codice di stato HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-204">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="4a2c1-205">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="4a2c1-205">Prerequisites</span></span>

<span data-ttu-id="4a2c1-206">Installare gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-206">Install the following:</span></span>
* <span data-ttu-id="4a2c1-207">Il pacchetto npm routing lato client.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-207">The client-side routing npm package.</span></span> <span data-ttu-id="4a2c1-208">Utilizzo di Angular ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-208">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="4a2c1-209">Configurazione</span><span class="sxs-lookup"><span data-stu-id="4a2c1-209">Configuration</span></span>

<span data-ttu-id="4a2c1-210">Un metodo di estensione denominato `MapSpaFallbackRoute` viene utilizzata la `Configure` metodo:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-210">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

<span data-ttu-id="4a2c1-211">Suggerimento: Le route vengono valutate nell'ordine in cui vengono configurate.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-211">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="4a2c1-212">Di conseguenza, il `default` route nell'esempio di codice precedente prima viene utilizzata per criteri di ricerca.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-212">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="4a2c1-213">Crea un nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="4a2c1-213">Creating a new project</span></span>

<span data-ttu-id="4a2c1-214">JavaScriptServices fornisce modelli di applicazione configurati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-214">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="4a2c1-215">SpaServices viene usato in questi modelli, insieme a diversi Framework e librerie, ad esempio Angular, React e Redux.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-215">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, React, and Redux.</span></span>

<span data-ttu-id="4a2c1-216">Questi modelli possono essere installati tramite la CLI di .NET Core, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-216">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="4a2c1-217">Viene visualizzato un elenco dei modelli di applicazione a singola pagina disponibili:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-217">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="4a2c1-218">Modelli</span><span class="sxs-lookup"><span data-stu-id="4a2c1-218">Templates</span></span>                                 | <span data-ttu-id="4a2c1-219">Nome breve</span><span class="sxs-lookup"><span data-stu-id="4a2c1-219">Short Name</span></span> | <span data-ttu-id="4a2c1-220">Linguaggio</span><span class="sxs-lookup"><span data-stu-id="4a2c1-220">Language</span></span> | <span data-ttu-id="4a2c1-221">Tag</span><span class="sxs-lookup"><span data-stu-id="4a2c1-221">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="4a2c1-222">MVC ASP.NET Core con Angular</span><span class="sxs-lookup"><span data-stu-id="4a2c1-222">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="4a2c1-223">angular</span><span class="sxs-lookup"><span data-stu-id="4a2c1-223">angular</span></span>    | <span data-ttu-id="4a2c1-224">[C#]</span><span class="sxs-lookup"><span data-stu-id="4a2c1-224">[C#]</span></span>     | <span data-ttu-id="4a2c1-225">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="4a2c1-225">Web/MVC/SPA</span></span> |
| <span data-ttu-id="4a2c1-226">MVC ASP.NET Core con React. js</span><span class="sxs-lookup"><span data-stu-id="4a2c1-226">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="4a2c1-227">react</span><span class="sxs-lookup"><span data-stu-id="4a2c1-227">react</span></span>      | <span data-ttu-id="4a2c1-228">[C#]</span><span class="sxs-lookup"><span data-stu-id="4a2c1-228">[C#]</span></span>     | <span data-ttu-id="4a2c1-229">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="4a2c1-229">Web/MVC/SPA</span></span> |
| <span data-ttu-id="4a2c1-230">MVC ASP.NET Core con React.js e Redux</span><span class="sxs-lookup"><span data-stu-id="4a2c1-230">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="4a2c1-231">reactredux</span><span class="sxs-lookup"><span data-stu-id="4a2c1-231">reactredux</span></span> | <span data-ttu-id="4a2c1-232">[C#]</span><span class="sxs-lookup"><span data-stu-id="4a2c1-232">[C#]</span></span>     | <span data-ttu-id="4a2c1-233">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="4a2c1-233">Web/MVC/SPA</span></span> |

<span data-ttu-id="4a2c1-234">Per creare un nuovo progetto usando uno dei modelli di applicazione a singola pagina, inclusa la **nome breve** del modello nel [dotnet nuovo](/dotnet/core/tools/dotnet-new) comando.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-234">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="4a2c1-235">Il comando seguente crea un'applicazione Angular con ASP.NET Core MVC configurata per il lato server:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-235">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="4a2c1-236">Impostare la modalità di configurazione di runtime</span><span class="sxs-lookup"><span data-stu-id="4a2c1-236">Set the runtime configuration mode</span></span>

<span data-ttu-id="4a2c1-237">Sono disponibili due modalità di configurazione di runtime principale:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-237">Two primary runtime configuration modes exist:</span></span>
* <span data-ttu-id="4a2c1-238">**Sviluppo**:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-238">**Development**:</span></span>
    * <span data-ttu-id="4a2c1-239">Include i mapping di origine a fini di debug.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-239">Includes source maps to ease debugging.</span></span>
    * <span data-ttu-id="4a2c1-240">Non ottimizzare il codice lato client per le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-240">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="4a2c1-241">**Ambiente di produzione**:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-241">**Production**:</span></span>
    * <span data-ttu-id="4a2c1-242">Esclude il mapping di origine.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-242">Excludes source maps.</span></span>
    * <span data-ttu-id="4a2c1-243">Ottimizza il codice lato client tramite creazione di bundle e minimizzazione.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-243">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="4a2c1-244">ASP.NET Core Usa variabile di ambiente denominata `ASPNETCORE_ENVIRONMENT` per archiviare la modalità di configurazione.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-244">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="4a2c1-245">Visualizzare **[impostare l'ambiente](xref:fundamentals/environments#set-the-environment)** per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-245">See **[Set the environment](xref:fundamentals/environments#set-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="4a2c1-246">In esecuzione con .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="4a2c1-246">Running with .NET Core CLI</span></span>

<span data-ttu-id="4a2c1-247">Ripristinare i pacchetti npm e NuGet necessari eseguendo il comando seguente nella radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-247">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="4a2c1-248">Compilare ed eseguire l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-248">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="4a2c1-249">Avvio dell'applicazione sull'host locale in base al [modalità di configurazione di runtime](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="4a2c1-249">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="4a2c1-250">Passare a `http://localhost:5000` nel browser verrà visualizzata la pagina di destinazione.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-250">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="4a2c1-251">In esecuzione con Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="4a2c1-251">Running with Visual Studio 2017</span></span>

<span data-ttu-id="4a2c1-252">Aprire il *file con estensione csproj* file generato dal [dotnet nuovo](/dotnet/core/tools/dotnet-new) comando.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-252">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="4a2c1-253">I pacchetti NuGet e npm necessari vengono ripristinati automaticamente al progetto aperto.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-253">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="4a2c1-254">Questo processo di ripristino può richiedere alcuni minuti e l'applicazione è pronta per l'esecuzione quando viene completato.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-254">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="4a2c1-255">Fare clic sul pulsante di esecuzione verde o premere `Ctrl + F5`, e il browser si apre alla pagina di destinazione dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-255">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="4a2c1-256">L'applicazione viene eseguita sull'host locale in base al [modalità di configurazione di runtime](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="4a2c1-256">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> 

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="4a2c1-257">Test dell'app</span><span class="sxs-lookup"><span data-stu-id="4a2c1-257">Testing the app</span></span>

<span data-ttu-id="4a2c1-258">I modelli SpaServices sono preconfigurati per eseguire i test sul lato client, usando [Karma](https://karma-runner.github.io/1.0/index.html) e [Jasmine](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="4a2c1-258">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="4a2c1-259">Jasmine è un diffuso framework di unit test per JavaScript, mentre Karma è un test runner per tali test.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-259">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="4a2c1-260">Karma è configurato per funzionare con il [Webpack Dev Middleware](#webpack-dev-middleware) tale che lo sviluppatore non è necessario arrestare ed eseguire i test ogni volta che vengono apportate modifiche.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-260">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="4a2c1-261">Se si tratta del codice in esecuzione per il test case o test case stesso, il test viene eseguito automaticamente.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-261">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="4a2c1-262">Usa l'applicazione Angular ad esempio, due Jasmine test case sono già fornite per il `CounterComponent` nella *counter.component.spec.ts* file:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-262">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="4a2c1-263">Aprire il prompt dei comandi nel *ClientApp* directory.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-263">Open the command prompt in the *ClientApp* directory.</span></span> <span data-ttu-id="4a2c1-264">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-264">Run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="4a2c1-265">Lo script avvia il test runner Karma, nel quale vengono lette le impostazioni definite nel *karma.conf.js* file.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-265">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="4a2c1-266">Tra le altre impostazioni, il *karma.conf.js* identifica i file di test da eseguire tramite relativo `files` matrice:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-266">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="4a2c1-267">Pubblicazione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="4a2c1-267">Publishing the application</span></span>

<span data-ttu-id="4a2c1-268">La combinazione degli asset generati dal lato client e gli elementi pubblicati in ASP.NET Core in un pacchetto pronto per la distribuzione può essere complessa.</span><span class="sxs-lookup"><span data-stu-id="4a2c1-268">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="4a2c1-269">Per fortuna, SpaServices Orchestra processo intera pubblicazione con una destinazione di MSBuild personalizzata denominata `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-269">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="4a2c1-270">La destinazione MSBuild ha le responsabilità seguenti:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-270">The MSBuild target has the following responsibilities:</span></span>
1. <span data-ttu-id="4a2c1-271">Ripristinare i pacchetti npm</span><span class="sxs-lookup"><span data-stu-id="4a2c1-271">Restore the npm packages</span></span>
1. <span data-ttu-id="4a2c1-272">Creare una build di livello di produzione gli asset lato client, di terze parti</span><span class="sxs-lookup"><span data-stu-id="4a2c1-272">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="4a2c1-273">Creare una build di livello di produzione di risorse lato client personalizzate</span><span class="sxs-lookup"><span data-stu-id="4a2c1-273">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="4a2c1-274">Copiare gli asset Webpack generato nella cartella di pubblicazione</span><span class="sxs-lookup"><span data-stu-id="4a2c1-274">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="4a2c1-275">La destinazione MSBuild viene richiamata durante l'esecuzione:</span><span class="sxs-lookup"><span data-stu-id="4a2c1-275">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="4a2c1-276">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4a2c1-276">Additional resources</span></span>

* [<span data-ttu-id="4a2c1-277">Documentazione di Angular</span><span class="sxs-lookup"><span data-stu-id="4a2c1-277">Angular Docs</span></span>](https://angular.io/docs)
