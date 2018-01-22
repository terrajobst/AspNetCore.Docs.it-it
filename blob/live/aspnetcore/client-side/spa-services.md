---
title: Utilizzo di JavaScriptServices per la creazione di applicazioni a pagina singola
author: scottaddie
description: Informazioni sui vantaggi dell'utilizzo JavaScriptServices per creare un'applicazione a pagina singola (SPA) supportato da ASP.NET Core.
ms.author: scaddie
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/spa-services
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6d84659c8c65bebb46551eb38bd52e405ff56016
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="using-javascriptservices-for-creating-single-page-applications-with-aspnet-core"></a><span data-ttu-id="e372f-103">Utilizzo di JavaScriptServices per la creazione di applicazioni a pagina singola con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e372f-103">Using JavaScriptServices for Creating Single Page Applications with ASP.NET Core</span></span>

<span data-ttu-id="e372f-104">Da [Scott Addie](https://github.com/scottaddie) e [Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="e372f-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="e372f-105">Un'applicazione a pagina singola (SPA) è un tipo comune di applicazione web a causa l'esperienza utente avanzata inerente.</span><span class="sxs-lookup"><span data-stu-id="e372f-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="e372f-106">Integrazione Framework SPA o raccolte, sul lato client, ad esempio [angolare](https://angular.io/) o [reagire](https://facebook.github.io/react/), con Framework sul lato server quali ASP.NET Core può essere difficile.</span><span class="sxs-lookup"><span data-stu-id="e372f-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="e372f-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) è stato sviluppato per ridurre le forze di attrito nel processo di integrazione.</span><span class="sxs-lookup"><span data-stu-id="e372f-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="e372f-108">In questo modo l'operatività tra diversi client e gli stack di tecnologie di server.</span><span class="sxs-lookup"><span data-stu-id="e372f-108">It enables seamless operation between the different client and server technology stacks.</span></span>

<span data-ttu-id="e372f-109">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e372f-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="e372f-110">Che cos'è JavaScriptServices?</span><span class="sxs-lookup"><span data-stu-id="e372f-110">What is JavaScriptServices?</span></span>

<span data-ttu-id="e372f-111">JavaScriptServices è una raccolta di tecnologie lato client per ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e372f-111">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="e372f-112">L'obiettivo consiste nel posizionare ASP.NET Core come piattaforma sul lato server preferita degli sviluppatori per la compilazione di stabilimenti termali.</span><span class="sxs-lookup"><span data-stu-id="e372f-112">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="e372f-113">JavaScriptServices è costituita da tre pacchetti NuGet distinti:</span><span class="sxs-lookup"><span data-stu-id="e372f-113">JavaScriptServices consists of three distinct NuGet packages:</span></span>
* <span data-ttu-id="e372f-114">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="e372f-114">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="e372f-115">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="e372f-115">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="e372f-116">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="e372f-116">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="e372f-117">Questi pacchetti sono utili se si:</span><span class="sxs-lookup"><span data-stu-id="e372f-117">These packages are useful if you:</span></span>
* <span data-ttu-id="e372f-118">Eseguire JavaScript nel server</span><span class="sxs-lookup"><span data-stu-id="e372f-118">Run JavaScript on the server</span></span>
* <span data-ttu-id="e372f-119">Utilizzare un framework SPA o una raccolta</span><span class="sxs-lookup"><span data-stu-id="e372f-119">Use a SPA framework or library</span></span>
* <span data-ttu-id="e372f-120">Compilazione di risorse sul lato client con Webpack</span><span class="sxs-lookup"><span data-stu-id="e372f-120">Build client-side assets with Webpack</span></span>

<span data-ttu-id="e372f-121">Gran parte dello stato attivo in questo articolo viene posizionata sull'uso del pacchetto SpaServices.</span><span class="sxs-lookup"><span data-stu-id="e372f-121">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="e372f-122">Che cos'è SpaServices?</span><span class="sxs-lookup"><span data-stu-id="e372f-122">What is SpaServices?</span></span>

<span data-ttu-id="e372f-123">SpaServices è stato creato per posizionare ASP.NET Core come piattaforma sul lato server preferita degli sviluppatori per la compilazione di stabilimenti termali.</span><span class="sxs-lookup"><span data-stu-id="e372f-123">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="e372f-124">SpaServices non è necessario sviluppare stabilimenti termali con ASP.NET Core, e non è del blocco in un framework client specifico.</span><span class="sxs-lookup"><span data-stu-id="e372f-124">SpaServices is not required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="e372f-125">SpaServices fornisce infrastruttura utile, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e372f-125">SpaServices provides useful infrastructure such as:</span></span>
* [<span data-ttu-id="e372f-126">Rendering preliminare sul lato server</span><span class="sxs-lookup"><span data-stu-id="e372f-126">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="e372f-127">Dev Middleware Webpack</span><span class="sxs-lookup"><span data-stu-id="e372f-127">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="e372f-128">Sostituzione di un modulo a caldo</span><span class="sxs-lookup"><span data-stu-id="e372f-128">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="e372f-129">Helper di routing</span><span class="sxs-lookup"><span data-stu-id="e372f-129">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="e372f-130">Complessivamente, questi componenti dell'infrastruttura migliorano sia il flusso di lavoro di sviluppo e il runtime.</span><span class="sxs-lookup"><span data-stu-id="e372f-130">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="e372f-131">I componenti possono essere adottati singolarmente.</span><span class="sxs-lookup"><span data-stu-id="e372f-131">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="e372f-132">Prerequisiti per l'utilizzo di SpaServices</span><span class="sxs-lookup"><span data-stu-id="e372f-132">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="e372f-133">Per utilizzare SpaServices, installare quanto segue:</span><span class="sxs-lookup"><span data-stu-id="e372f-133">To work with SpaServices, install the following:</span></span>
* <span data-ttu-id="e372f-134">[Node.js](https://nodejs.org/) (versione 6 o versione successiva) con npm</span><span class="sxs-lookup"><span data-stu-id="e372f-134">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
    * <span data-ttu-id="e372f-135">Per verificare questi componenti sono installati ed sono disponibile, eseguire il comando seguente dalla riga di comando:</span><span class="sxs-lookup"><span data-stu-id="e372f-135">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="e372f-136">Nota: Se si distribuisce in un sito web di Azure, non è necessario effettuare alcuna operazione qui &mdash; Node.js è installato e disponibile negli ambienti server.</span><span class="sxs-lookup"><span data-stu-id="e372f-136">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* <span data-ttu-id="e372f-137">[.NET core SDK](https://www.microsoft.com/net/download/core) 1.0 (o versioni successive)</span><span class="sxs-lookup"><span data-stu-id="e372f-137">[.NET Core SDK](https://www.microsoft.com/net/download/core) 1.0 (or later)</span></span>
    * <span data-ttu-id="e372f-138">Se si è in Windows, può essere installato, selezionare Visual Studio 2017 **lo sviluppo multipiattaforma con .NET Core** carico di lavoro.</span><span class="sxs-lookup"><span data-stu-id="e372f-138">If you're on Windows, this can be installed by selecting Visual Studio 2017's **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="e372f-139">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) pacchetto NuGet</span><span class="sxs-lookup"><span data-stu-id="e372f-139">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="e372f-140">Rendering preliminare sul lato server</span><span class="sxs-lookup"><span data-stu-id="e372f-140">Server-side prerendering</span></span>

<span data-ttu-id="e372f-141">Un'applicazione universale (noto anche come siano isomorfi) è un'applicazione JavaScript in grado di eseguire sia sul server e client.</span><span class="sxs-lookup"><span data-stu-id="e372f-141">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="e372f-142">Angolare React e altri framework comuni offre una piattaforma universale per questo stile di sviluppo di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="e372f-142">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="e372f-143">L'idea è prima di eseguire il rendering i componenti del framework sul server tramite Node.js e quindi delegare ulteriormente l'esecuzione del client.</span><span class="sxs-lookup"><span data-stu-id="e372f-143">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="e372f-144">ASP.NET Core [gli helper di Tag](xref:mvc/views/tag-helpers/intro) fornito da SpaServices semplifica l'implementazione del rendering preliminare sul lato server chiamando le funzioni JavaScript nel server.</span><span class="sxs-lookup"><span data-stu-id="e372f-144">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="e372f-145">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="e372f-145">Prerequisites</span></span>

<span data-ttu-id="e372f-146">Installare gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="e372f-146">Install the following:</span></span>
* <span data-ttu-id="e372f-147">[il rendering preliminare di ASPNET](https://www.npmjs.com/package/aspnet-prerendering) pacchetto npm:</span><span class="sxs-lookup"><span data-stu-id="e372f-147">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="e372f-148">Configurazione</span><span class="sxs-lookup"><span data-stu-id="e372f-148">Configuration</span></span>

<span data-ttu-id="e372f-149">Gli helper di Tag resi individuabili tramite la registrazione dello spazio dei nomi del progetto *viewimports. cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="e372f-149">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="e372f-150">Questi helper di Tag di astrarre le complessità di comunicare direttamente con l'API di basso livello tramite una sintassi simile a HTML all'interno della visualizzazione Razor:</span><span class="sxs-lookup"><span data-stu-id="e372f-150">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="e372f-151">Il `asp-prerender-module` Helper Tag</span><span class="sxs-lookup"><span data-stu-id="e372f-151">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="e372f-152">Il `asp-prerender-module` Helper di Tag, utilizzato nell'esempio di codice precedente, esegue *ClientApp/dist/main-server.js* sul server tramite Node.js.</span><span class="sxs-lookup"><span data-stu-id="e372f-152">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="e372f-153">Per chiarezza, *main server.js* file è un elemento dell'attività transpilation TypeScript e JavaScript il [Webpack](http://webpack.github.io/) processo di compilazione.</span><span class="sxs-lookup"><span data-stu-id="e372f-153">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="e372f-154">Webpack definisce un alias di punto di ingresso di `main-server`; e l'attraversamento di grafico delle dipendenze per questo alias inizia in corrispondenza di *ClientApp/avvio-server.ts* file:</span><span class="sxs-lookup"><span data-stu-id="e372f-154">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="e372f-155">Nell'esempio seguente angolare il *ClientApp/avvio-server.ts* file utilizza il `createServerRenderer` (funzione) e `RenderResult` tipo del `aspnet-prerendering` pacchetto npm da configurare per il rendering di server tramite Node.js.</span><span class="sxs-lookup"><span data-stu-id="e372f-155">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="e372f-156">Il markup HTML destinato per il rendering lato server viene passato a una chiamata di funzione di risoluzione, viene eseguito il wrapping in un JavaScript fortemente tipizzato `Promise` oggetto.</span><span class="sxs-lookup"><span data-stu-id="e372f-156">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="e372f-157">Il `Promise` è di importanza dell'oggetto in modo asincrono che fornisce il markup HTML per la pagina per l'aggiunta nell'elemento segnaposto del DOM.</span><span class="sxs-lookup"><span data-stu-id="e372f-157">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="e372f-158">Il `asp-prerender-data` Helper Tag</span><span class="sxs-lookup"><span data-stu-id="e372f-158">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="e372f-159">Quando è associato il `asp-prerender-module` Helper di Tag, il `asp-prerender-data` Helper di Tag può essere usato per passare informazioni contestuali dalla visualizzazione Razor per il codice JavaScript sul lato server.</span><span class="sxs-lookup"><span data-stu-id="e372f-159">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="e372f-160">Ad esempio, il markup seguente passa i dati utente per il `main-server` modulo:</span><span class="sxs-lookup"><span data-stu-id="e372f-160">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="e372f-161">I dati ricevuti `UserName` argomento viene serializzato utilizzando il serializzatore JSON incorporato e viene archiviato nel `params.data` oggetto.</span><span class="sxs-lookup"><span data-stu-id="e372f-161">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="e372f-162">Nell'esempio seguente angolare, i dati vengono utilizzati per costruire un messaggio di saluto personalizzato all'interno di un `h1` elemento:</span><span class="sxs-lookup"><span data-stu-id="e372f-162">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="e372f-163">Nota: I nomi delle proprietà passati gli helper di Tag sono rappresentati con **PascalCase** notazione.</span><span class="sxs-lookup"><span data-stu-id="e372f-163">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="e372f-164">Invece che a JavaScript, in cui gli stessi nomi di proprietà sono rappresentati con **camelCase**.</span><span class="sxs-lookup"><span data-stu-id="e372f-164">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="e372f-165">La configurazione predefinita della serializzazione JSON è responsabile di questa differenza.</span><span class="sxs-lookup"><span data-stu-id="e372f-165">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="e372f-166">Per espandere l'esempio di codice precedente, dati possono essere passati dal server per la visualizzazione da idratare le il `globals` proprietà fornita per il `resolve` funzione:</span><span class="sxs-lookup"><span data-stu-id="e372f-166">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="e372f-167">Il `postList` matrice definita all'interno di `globals` oggetto viene connesso al browser globale `window` oggetto.</span><span class="sxs-lookup"><span data-stu-id="e372f-167">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="e372f-168">Questa variabile sottraendo in ambito globale elimina la duplicazione di impegno, in particolare per quanto attiene per il caricamento dei dati stessi, una volta sul server e di nuovo nel client.</span><span class="sxs-lookup"><span data-stu-id="e372f-168">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![variabile postList globale associata all'oggetto finestra](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="e372f-170">Dev Middleware Webpack</span><span class="sxs-lookup"><span data-stu-id="e372f-170">Webpack Dev Middleware</span></span>

<span data-ttu-id="e372f-171">[Dev Middleware Webpack](https://webpack.github.io/docs/webpack-dev-middleware.html) introduce un flusso di lavoro di sviluppo ottimizzato in base al quale Webpack compila le risorse su richiesta.</span><span class="sxs-lookup"><span data-stu-id="e372f-171">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="e372f-172">Il middleware automaticamente compila e serve le risorse sul lato client quando una pagina viene ricaricata nel browser.</span><span class="sxs-lookup"><span data-stu-id="e372f-172">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="e372f-173">L'approccio alternativo consiste nel richiamare manualmente Webpack tramite script di compilazione del progetto npm quando viene modificato il codice personalizzato o una dipendenza di terze parti.</span><span class="sxs-lookup"><span data-stu-id="e372f-173">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="e372f-174">Script di compilazione un npm *package. JSON* file è illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="e372f-174">An npm build script in the *package.json* file is shown in the following example:</span></span>

[!code-json[Main](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a><span data-ttu-id="e372f-175">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="e372f-175">Prerequisites</span></span>

<span data-ttu-id="e372f-176">Installare gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="e372f-176">Install the following:</span></span>
* <span data-ttu-id="e372f-177">[ASPNET webpack](https://www.npmjs.com/package/aspnet-webpack) pacchetto npm:</span><span class="sxs-lookup"><span data-stu-id="e372f-177">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="e372f-178">Configurazione</span><span class="sxs-lookup"><span data-stu-id="e372f-178">Configuration</span></span>

<span data-ttu-id="e372f-179">Dev Middleware Webpack è registrato nella pipeline delle richieste HTTP tramite il codice seguente nel *Startup.cs* file `Configure` metodo:</span><span class="sxs-lookup"><span data-stu-id="e372f-179">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

<span data-ttu-id="e372f-180">Il `UseWebpackDevMiddleware` metodo di estensione deve essere chiamato prima [la registrazione di file statici hosting](xref:fundamentals/static-files) tramite il `UseStaticFiles` metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="e372f-180">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="e372f-181">Per motivi di sicurezza, è possibile registrare il middleware solo quando l'app in esecuzione in modalità di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="e372f-181">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="e372f-182">Il *webpack.config.js* file `output.publicPath` proprietà indica il middleware per controllare il `dist` cartella per le modifiche:</span><span class="sxs-lookup"><span data-stu-id="e372f-182">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="e372f-183">Sostituzione di un modulo a caldo</span><span class="sxs-lookup"><span data-stu-id="e372f-183">Hot Module Replacement</span></span>

<span data-ttu-id="e372f-184">Si consideri di Webpack [sostituzione di un modulo a caldo](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) funzionalità (HMR) come un'evoluzione del [Webpack Dev Middleware](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="e372f-184">Think of Webpack's [Hot Module Replacement](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="e372f-185">HMR introduce gli stessi vantaggi, ma ulteriormente semplifica il flusso di lavoro di sviluppo aggiornando automaticamente il contenuto della pagina dopo la compilazione delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="e372f-185">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="e372f-186">Non confondere con un aggiornamento del browser, che potrebbe interferire con la stato in memoria corrente e la sessione di debug di SPA.</span><span class="sxs-lookup"><span data-stu-id="e372f-186">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="e372f-187">È presente un collegamento in tempo reale tra il servizio Webpack Dev Middleware e il browser, il che significa che le modifiche vengono inviate al browser.</span><span class="sxs-lookup"><span data-stu-id="e372f-187">There is a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="e372f-188">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="e372f-188">Prerequisites</span></span>

<span data-ttu-id="e372f-189">Installare gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="e372f-189">Install the following:</span></span>
* <span data-ttu-id="e372f-190">[middleware hot webpack](https://www.npmjs.com/package/webpack-hot-middleware) pacchetto npm:</span><span class="sxs-lookup"><span data-stu-id="e372f-190">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="e372f-191">Configurazione</span><span class="sxs-lookup"><span data-stu-id="e372f-191">Configuration</span></span>

<span data-ttu-id="e372f-192">Il componente HMR deve essere registrato in pipeline di richieste HTTP di MVC nel `Configure` metodo:</span><span class="sxs-lookup"><span data-stu-id="e372f-192">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="e372f-193">Come è stato true con [Webpack Dev Middleware](#webpack-dev-middleware), `UseWebpackDevMiddleware` metodo di estensione deve essere chiamato prima il `UseStaticFiles` metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="e372f-193">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="e372f-194">Per motivi di sicurezza, è possibile registrare il middleware solo quando l'app in esecuzione in modalità di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="e372f-194">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="e372f-195">Il *webpack.config.js* file è necessario definire un `plugins` matrice, anche se viene lasciata vuota:</span><span class="sxs-lookup"><span data-stu-id="e372f-195">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="e372f-196">Dopo il caricamento dell'app nel browser, scheda Console gli strumenti di sviluppo fornisce conferma dell'attivazione HMR:</span><span class="sxs-lookup"><span data-stu-id="e372f-196">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![Messaggio di connessione a caldo di sostituzione di un modulo](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="e372f-198">Helper di routing</span><span class="sxs-lookup"><span data-stu-id="e372f-198">Routing helpers</span></span>

<span data-ttu-id="e372f-199">Nella maggior parte dei stabilimenti termali basato su ASP.NET Core, è opportuno routing sul lato client oltre al routing sul lato server.</span><span class="sxs-lookup"><span data-stu-id="e372f-199">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="e372f-200">I sistemi di routing SPA e MVC possono lavorare in modo indipendente senza interferenze.</span><span class="sxs-lookup"><span data-stu-id="e372f-200">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="e372f-201">È, tuttavia, un caso limite ponendo sfide: identificazione delle risposte HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="e372f-201">There is, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="e372f-202">Si consideri lo scenario in cui una route senza estensione di `/some/page` viene utilizzato.</span><span class="sxs-lookup"><span data-stu-id="e372f-202">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="e372f-203">Si supponga che la richiesta non-corrispondenza una route sul lato server, ma il criterio di corrispondenza con una route sul lato client.</span><span class="sxs-lookup"><span data-stu-id="e372f-203">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="e372f-204">Ora si consideri una richiesta in ingresso per `/images/user-512.png`, che in genere prevede di trovare un file di immagine nel server.</span><span class="sxs-lookup"><span data-stu-id="e372f-204">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="e372f-205">Se il percorso della risorsa richiesta non corrisponde a qualsiasi route sul lato server o un file statico, è improbabile che l'applicazione sul lato client si gestisce, in genere si desidera restituire un codice di stato HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="e372f-205">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="e372f-206">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="e372f-206">Prerequisites</span></span>

<span data-ttu-id="e372f-207">Installare gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="e372f-207">Install the following:</span></span>
* <span data-ttu-id="e372f-208">Il pacchetto npm routing sul lato client.</span><span class="sxs-lookup"><span data-stu-id="e372f-208">The client-side routing npm package.</span></span> <span data-ttu-id="e372f-209">Ad esempio, utilizzando angolare:</span><span class="sxs-lookup"><span data-stu-id="e372f-209">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="e372f-210">Configurazione</span><span class="sxs-lookup"><span data-stu-id="e372f-210">Configuration</span></span>

<span data-ttu-id="e372f-211">Un metodo di estensione denominato `MapSpaFallbackRoute` viene utilizzata la `Configure` metodo:</span><span class="sxs-lookup"><span data-stu-id="e372f-211">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

<span data-ttu-id="e372f-212">Suggerimento: Le route vengono valutate nell'ordine in cui la configurazione di tali.</span><span class="sxs-lookup"><span data-stu-id="e372f-212">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="e372f-213">Di conseguenza, il `default` route nell'esempio di codice precedente prima viene utilizzata per criteri di ricerca.</span><span class="sxs-lookup"><span data-stu-id="e372f-213">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="e372f-214">Crea un nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="e372f-214">Creating a new project</span></span>

<span data-ttu-id="e372f-215">JavaScriptServices fornisce modelli di applicazione configurati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="e372f-215">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="e372f-216">SpaServices viene usato in questi modelli, insieme a diversi Framework e librerie, ad esempio angolare, Aurelia, Knockout, React e dotato.</span><span class="sxs-lookup"><span data-stu-id="e372f-216">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, Aurelia, Knockout, React, and Vue.</span></span>

<span data-ttu-id="e372f-217">Questi modelli possono essere installati tramite l'interfaccia CLI Core .NET eseguendo il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e372f-217">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="e372f-218">Viene visualizzato un elenco dei modelli SPA disponibili:</span><span class="sxs-lookup"><span data-stu-id="e372f-218">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="e372f-219">Modelli</span><span class="sxs-lookup"><span data-stu-id="e372f-219">Templates</span></span>                                 | <span data-ttu-id="e372f-220">Nome breve</span><span class="sxs-lookup"><span data-stu-id="e372f-220">Short Name</span></span> | <span data-ttu-id="e372f-221">Linguaggio</span><span class="sxs-lookup"><span data-stu-id="e372f-221">Language</span></span> | <span data-ttu-id="e372f-222">Tag</span><span class="sxs-lookup"><span data-stu-id="e372f-222">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="e372f-223">MVC ASP.NET Core con Angular</span><span class="sxs-lookup"><span data-stu-id="e372f-223">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="e372f-224">angular</span><span class="sxs-lookup"><span data-stu-id="e372f-224">angular</span></span>    | <span data-ttu-id="e372f-225">[C#]</span><span class="sxs-lookup"><span data-stu-id="e372f-225">[C#]</span></span>     | <span data-ttu-id="e372f-226">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="e372f-226">Web/MVC/SPA</span></span> |
| <span data-ttu-id="e372f-227">MVC ASP.NET Core con Aurelia</span><span class="sxs-lookup"><span data-stu-id="e372f-227">MVC ASP.NET Core with Aurelia</span></span>             | <span data-ttu-id="e372f-228">aurelia</span><span class="sxs-lookup"><span data-stu-id="e372f-228">aurelia</span></span>    | <span data-ttu-id="e372f-229">[C#]</span><span class="sxs-lookup"><span data-stu-id="e372f-229">[C#]</span></span>     | <span data-ttu-id="e372f-230">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="e372f-230">Web/MVC/SPA</span></span> |
| <span data-ttu-id="e372f-231">MVC ASP.NET Core con Knockout.js</span><span class="sxs-lookup"><span data-stu-id="e372f-231">MVC ASP.NET Core with Knockout.js</span></span>         | <span data-ttu-id="e372f-232">knockout</span><span class="sxs-lookup"><span data-stu-id="e372f-232">knockout</span></span>   | <span data-ttu-id="e372f-233">[C#]</span><span class="sxs-lookup"><span data-stu-id="e372f-233">[C#]</span></span>     | <span data-ttu-id="e372f-234">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="e372f-234">Web/MVC/SPA</span></span> |
| <span data-ttu-id="e372f-235">MVC ASP.NET Core con React.js</span><span class="sxs-lookup"><span data-stu-id="e372f-235">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="e372f-236">react</span><span class="sxs-lookup"><span data-stu-id="e372f-236">react</span></span>      | <span data-ttu-id="e372f-237">[C#]</span><span class="sxs-lookup"><span data-stu-id="e372f-237">[C#]</span></span>     | <span data-ttu-id="e372f-238">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="e372f-238">Web/MVC/SPA</span></span> |
| <span data-ttu-id="e372f-239">MVC ASP.NET Core con React.js e il ritorno</span><span class="sxs-lookup"><span data-stu-id="e372f-239">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="e372f-240">reactredux</span><span class="sxs-lookup"><span data-stu-id="e372f-240">reactredux</span></span> | <span data-ttu-id="e372f-241">[C#]</span><span class="sxs-lookup"><span data-stu-id="e372f-241">[C#]</span></span>     | <span data-ttu-id="e372f-242">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="e372f-242">Web/MVC/SPA</span></span> |
| <span data-ttu-id="e372f-243">MVC ASP.NET Core con Vue.js</span><span class="sxs-lookup"><span data-stu-id="e372f-243">MVC ASP.NET Core with Vue.js</span></span>              | <span data-ttu-id="e372f-244">dotato</span><span class="sxs-lookup"><span data-stu-id="e372f-244">vue</span></span>        | <span data-ttu-id="e372f-245">[C#]</span><span class="sxs-lookup"><span data-stu-id="e372f-245">[C#]</span></span>     | <span data-ttu-id="e372f-246">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="e372f-246">Web/MVC/SPA</span></span> | 

<span data-ttu-id="e372f-247">Per creare un nuovo progetto utilizzando uno dei modelli di SPA, inclusa la **nome breve** del modello nel `dotnet new` comando.</span><span class="sxs-lookup"><span data-stu-id="e372f-247">To create a new project using one of the SPA templates, include the **Short Name** of the template in the `dotnet new` command.</span></span> <span data-ttu-id="e372f-248">Il comando seguente crea un'applicazione angolare con ASP.NET MVC di base configurato per il lato server:</span><span class="sxs-lookup"><span data-stu-id="e372f-248">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="e372f-249">Impostare la modalità di configurazione di runtime</span><span class="sxs-lookup"><span data-stu-id="e372f-249">Set the runtime configuration mode</span></span>

<span data-ttu-id="e372f-250">Esistono due modalità di configurazione di runtime principale:</span><span class="sxs-lookup"><span data-stu-id="e372f-250">Two primary runtime configuration modes exist:</span></span>
* <span data-ttu-id="e372f-251">**Sviluppo**:</span><span class="sxs-lookup"><span data-stu-id="e372f-251">**Development**:</span></span>
    * <span data-ttu-id="e372f-252">Include i mapping di origine per facilitare il debug.</span><span class="sxs-lookup"><span data-stu-id="e372f-252">Includes source maps to ease debugging.</span></span>
    * <span data-ttu-id="e372f-253">Non ottimizzare il codice sul lato client per le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="e372f-253">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="e372f-254">**Produzione**:</span><span class="sxs-lookup"><span data-stu-id="e372f-254">**Production**:</span></span>
    * <span data-ttu-id="e372f-255">Esclude i mapping di origine.</span><span class="sxs-lookup"><span data-stu-id="e372f-255">Excludes source maps.</span></span>
    * <span data-ttu-id="e372f-256">Ottimizza il codice sul lato client tramite l'aggregazione e di riduzione.</span><span class="sxs-lookup"><span data-stu-id="e372f-256">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="e372f-257">ASP.NET Core utilizza una variabile di ambiente denominata `ASPNETCORE_ENVIRONMENT` per archiviare la modalità di configurazione.</span><span class="sxs-lookup"><span data-stu-id="e372f-257">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="e372f-258">Vedere  **[l'impostazione dell'ambiente](xref:fundamentals/environments#setting-the-environment)**  per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="e372f-258">See **[Setting the environment](xref:fundamentals/environments#setting-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="e372f-259">In esecuzione con .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="e372f-259">Running with .NET Core CLI</span></span>

<span data-ttu-id="e372f-260">Ripristinare il NuGet necessarie e i pacchetti npm eseguendo il comando seguente nella radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="e372f-260">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="e372f-261">Compilare ed eseguire l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="e372f-261">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="e372f-262">Avvio dell'applicazione su localhost in base al [modalità di configurazione di runtime](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="e372f-262">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="e372f-263">Passare a `http://localhost:5000` nel browser verrà visualizzata la pagina di destinazione.</span><span class="sxs-lookup"><span data-stu-id="e372f-263">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="e372f-264">In esecuzione con Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="e372f-264">Running with Visual Studio 2017</span></span>

<span data-ttu-id="e372f-265">Aprire il *csproj* file generato dal `dotnet new` comando.</span><span class="sxs-lookup"><span data-stu-id="e372f-265">Open the *.csproj* file generated by the `dotnet new` command.</span></span> <span data-ttu-id="e372f-266">I pacchetti NuGet e npm richiesti vengono ripristinati automaticamente al progetto aperto.</span><span class="sxs-lookup"><span data-stu-id="e372f-266">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="e372f-267">Questo processo di ripristino potrebbe richiedere alcuni minuti e l'applicazione è pronta per l'esecuzione quando viene completato.</span><span class="sxs-lookup"><span data-stu-id="e372f-267">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="e372f-268">Fare clic sul pulsante Esegui verde o premere `Ctrl + F5`, e il browser si apre alla pagina di destinazione dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e372f-268">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="e372f-269">L'applicazione viene eseguita in localhost in base al [modalità di configurazione di runtime](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="e372f-269">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> 

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="e372f-270">Test dell'app</span><span class="sxs-lookup"><span data-stu-id="e372f-270">Testing the app</span></span>

<span data-ttu-id="e372f-271">I modelli di SpaServices sono configurati in precedenza per eseguire test sul lato client tramite [Karma](https://karma-runner.github.io/1.0/index.html) e [Jasmine](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="e372f-271">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="e372f-272">Jasmine è un diffuso framework unit test per JavaScript, mentre Karma è un test runner per i test.</span><span class="sxs-lookup"><span data-stu-id="e372f-272">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="e372f-273">Karma è configurato per funzionare con il [Webpack Dev Middleware](#webpack-dev-middleware) in modo che non è necessario arrestare ed eseguire il test ogni volta che vengono apportate modifiche.</span><span class="sxs-lookup"><span data-stu-id="e372f-273">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that you don’t have to stop and run the test every time changes are made.</span></span> <span data-ttu-id="e372f-274">Se si tratta del codice in esecuzione per il test case o test case stesso, il test viene eseguito automaticamente.</span><span class="sxs-lookup"><span data-stu-id="e372f-274">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="e372f-275">Utilizzando l'applicazione angolare ad esempio, due casi di prova al gelsomino sono già forniti per il `CounterComponent` nel *counter.component.spec.ts* file:</span><span class="sxs-lookup"><span data-stu-id="e372f-275">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="e372f-276">Aprire il prompt dei comandi nella radice del progetto ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e372f-276">Open the command prompt at the project root, and run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="e372f-277">Lo script avvia il Karma test runner, che legge le impostazioni definite nel *karma.conf.js* file.</span><span class="sxs-lookup"><span data-stu-id="e372f-277">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="e372f-278">Tra le altre impostazioni, il *karma.conf.js* identifica i file di test da eseguire tramite il relativo `files` matrice:</span><span class="sxs-lookup"><span data-stu-id="e372f-278">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="e372f-279">Pubblicazione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="e372f-279">Publishing the application</span></span>

<span data-ttu-id="e372f-280">Combinando le risorse sul lato client generate e gli elementi di ASP.NET Core pubblicati in un pacchetto pronte per l'utilizzo può essere complesso.</span><span class="sxs-lookup"><span data-stu-id="e372f-280">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="e372f-281">Fortunatamente, SpaServices Orchestra processo intera pubblicazione con una destinazione di MSBuild personalizzata denominata `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="e372f-281">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[Main](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="e372f-282">La destinazione MSBuild ha le responsabilità seguenti:</span><span class="sxs-lookup"><span data-stu-id="e372f-282">The MSBuild target has the following responsibilities:</span></span>
1. <span data-ttu-id="e372f-283">Ripristinare i pacchetti npm</span><span class="sxs-lookup"><span data-stu-id="e372f-283">Restore the npm packages</span></span>
1. <span data-ttu-id="e372f-284">Crea una build di livello di produzione gli asset di terze parti, sul lato client</span><span class="sxs-lookup"><span data-stu-id="e372f-284">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="e372f-285">Creare una build di produzione di livello di risorse sul lato client personalizzate</span><span class="sxs-lookup"><span data-stu-id="e372f-285">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="e372f-286">Copiare le risorse Webpack generato nella cartella di pubblicazione</span><span class="sxs-lookup"><span data-stu-id="e372f-286">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="e372f-287">La destinazione MSBuild viene richiamata durante l'esecuzione:</span><span class="sxs-lookup"><span data-stu-id="e372f-287">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="e372f-288">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e372f-288">Additional resources</span></span>

* [<span data-ttu-id="e372f-289">Documenti Angular</span><span class="sxs-lookup"><span data-stu-id="e372f-289">Angular Docs</span></span>](https://angular.io/docs)
