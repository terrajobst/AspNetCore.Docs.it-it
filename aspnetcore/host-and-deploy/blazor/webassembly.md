---
title: Ospitare e distribuire ASP.NET Core Blazor webassembly
author: guardrex
description: Informazioni su come ospitare e distribuire un'app Blazor usando ASP.NET Core, le reti per la distribuzione di contenuti (CDN), i file server e le pagine di GitHub.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/11/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/webassembly
ms.openlocfilehash: 748ac9969134f4c89cc8c1235958dcc7ac1d1080
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434278"
---
# <a name="host-and-deploy-aspnet-core-opno-locblazor-webassembly"></a><span data-ttu-id="8509b-103">Ospitare e distribuire ASP.NET Core Blazor webassembly</span><span class="sxs-lookup"><span data-stu-id="8509b-103">Host and deploy ASP.NET Core Blazor WebAssembly</span></span>

<span data-ttu-id="8509b-104">Di [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="8509b-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="8509b-105">Con il [modello di hostingBlazor webassembly](xref:blazor/hosting-models#blazor-webassembly):</span><span class="sxs-lookup"><span data-stu-id="8509b-105">With the [Blazor WebAssembly hosting model](xref:blazor/hosting-models#blazor-webassembly):</span></span>

* <span data-ttu-id="8509b-106">L'app Blazor, le relative dipendenze e il Runtime .NET vengono scaricati nel browser.</span><span class="sxs-lookup"><span data-stu-id="8509b-106">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="8509b-107">L'app viene eseguita direttamente nel thread dell'interfaccia utente del browser.</span><span class="sxs-lookup"><span data-stu-id="8509b-107">The app is executed directly on the browser UI thread.</span></span>

<span data-ttu-id="8509b-108">Sono supportate le strategie di distribuzione seguenti:</span><span class="sxs-lookup"><span data-stu-id="8509b-108">The following deployment strategies are supported:</span></span>

* <span data-ttu-id="8509b-109">L'app Blazor viene gestita da un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8509b-109">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="8509b-110">Questa strategia viene trattata nella sezione [Distribuzione ospitata con ASP.NET Core](#hosted-deployment-with-aspnet-core).</span><span class="sxs-lookup"><span data-stu-id="8509b-110">This strategy is covered in the [Hosted deployment with ASP.NET Core](#hosted-deployment-with-aspnet-core) section.</span></span>
* <span data-ttu-id="8509b-111">L'app Blazor si trova in un server o un servizio Web di hosting statico, in cui .NET non viene usato per gestire l'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="8509b-111">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="8509b-112">Questa strategia è illustrata nella sezione relativa alla [distribuzione autonoma](#standalone-deployment) , che include informazioni sull'hosting di un'app webassembly Blazor come sottoapp IIS.</span><span class="sxs-lookup"><span data-stu-id="8509b-112">This strategy is covered in the [Standalone deployment](#standalone-deployment) section, which includes information on hosting a Blazor WebAssembly app as an IIS sub-app.</span></span>

## <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="8509b-113">Riscrivere gli URL per il routing corretto</span><span class="sxs-lookup"><span data-stu-id="8509b-113">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="8509b-114">Il routing delle richieste per i componenti della pagina in un'app webassembly Blazor non è semplice come le richieste di routing in un server Blazor, un'app ospitata.</span><span class="sxs-lookup"><span data-stu-id="8509b-114">Routing requests for page components in a Blazor WebAssembly app isn't as straightforward as routing requests in a Blazor Server, hosted app.</span></span> <span data-ttu-id="8509b-115">Si consideri un'app webassembly Blazor con due componenti:</span><span class="sxs-lookup"><span data-stu-id="8509b-115">Consider a Blazor WebAssembly app with two components:</span></span>

* <span data-ttu-id="8509b-116">*Main. razor* &ndash; carica alla radice dell'app e contiene un collegamento al componente di `About` (`href="About"`).</span><span class="sxs-lookup"><span data-stu-id="8509b-116">*Main.razor* &ndash; Loads at the root of the app and contains a link to the `About` component (`href="About"`).</span></span>
* <span data-ttu-id="8509b-117">*About. razor* &ndash; `About` componente.</span><span class="sxs-lookup"><span data-stu-id="8509b-117">*About.razor* &ndash; `About` component.</span></span>

<span data-ttu-id="8509b-118">Quando viene richiesto il documento predefinito dell'app usando la barra degli indirizzi del browser (ad esempio, `https://www.contoso.com/`):</span><span class="sxs-lookup"><span data-stu-id="8509b-118">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="8509b-119">Il browser invia una richiesta.</span><span class="sxs-lookup"><span data-stu-id="8509b-119">The browser makes a request.</span></span>
1. <span data-ttu-id="8509b-120">Viene restituita la pagina predefinita, di solito *index.html*.</span><span class="sxs-lookup"><span data-stu-id="8509b-120">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="8509b-121">*index.html* esegue il bootstrap dell'app.</span><span class="sxs-lookup"><span data-stu-id="8509b-121">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="8509b-122">il router di Blazorviene caricato e viene eseguito il rendering del componente `Main` Razor.</span><span class="sxs-lookup"><span data-stu-id="8509b-122">Blazor's router loads, and the Razor `Main` component is rendered.</span></span>

<span data-ttu-id="8509b-123">Nella pagina principale, selezionando il collegamento al componente `About` funziona sul client perché il router Blazor impedisce al browser di effettuare una richiesta su Internet per `www.contoso.com` per `About` e funge da `About` componente di cui è stato eseguito il rendering.</span><span class="sxs-lookup"><span data-stu-id="8509b-123">In the Main page, selecting the link to the `About` component works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the rendered `About` component itself.</span></span> <span data-ttu-id="8509b-124">Tutte le richieste per gli endpoint interni *all'interno dell'app Blazor webassembly* funzionano allo stesso modo: le richieste non attivano richieste basate su browser a risorse ospitate su server su Internet.</span><span class="sxs-lookup"><span data-stu-id="8509b-124">All of the requests for internal endpoints *within the Blazor WebAssembly app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="8509b-125">Le richieste vengono gestite internamente dal router.</span><span class="sxs-lookup"><span data-stu-id="8509b-125">The router handles the requests internally.</span></span>

<span data-ttu-id="8509b-126">Se una richiesta viene effettuata usando la barra degli indirizzi del browser per `www.contoso.com/About`, la richiesta ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="8509b-126">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="8509b-127">La risorsa non esiste nell'host Internet dell'app, quindi viene restituita la risposta *404 non trovato*.</span><span class="sxs-lookup"><span data-stu-id="8509b-127">No such resource exists on the app's Internet host, so a *404 - Not Found* response is returned.</span></span>

<span data-ttu-id="8509b-128">Dato che i browser inviano le richieste agli host basati su Internet per le pagine sul lato client, i server Web e i servizi di hosting devono riscrivere tutte le richieste per le risorse che non si trovano fisicamente nel server per la pagina *index.html*.</span><span class="sxs-lookup"><span data-stu-id="8509b-128">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="8509b-129">Quando viene restituito *index. html* , il router di Blazor dell'app accetta e risponde con la risorsa corretta.</span><span class="sxs-lookup"><span data-stu-id="8509b-129">When *index.html* is returned, the app's Blazor router takes over and responds with the correct resource.</span></span>

<span data-ttu-id="8509b-130">Quando si esegue la distribuzione in un server IIS, è possibile usare il modulo URL Rewrite con il file *Web. config* pubblicato dall'app.</span><span class="sxs-lookup"><span data-stu-id="8509b-130">When deploying to an IIS server, you can use the URL Rewrite Module with the app's published *web.config* file.</span></span> <span data-ttu-id="8509b-131">Per ulteriori informazioni, vedere la sezione [IIS](#iis) .</span><span class="sxs-lookup"><span data-stu-id="8509b-131">For more information, see the [IIS](#iis) section.</span></span>

## <a name="hosted-deployment-with-aspnet-core"></a><span data-ttu-id="8509b-132">Distribuzione ospitata con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8509b-132">Hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="8509b-133">Una *distribuzione ospitata* serve l'app webassembly Blazor ai browser da un' [app ASP.NET Core](xref:index) in esecuzione su un server Web.</span><span class="sxs-lookup"><span data-stu-id="8509b-133">A *hosted deployment* serves the Blazor WebAssembly app to browsers from an [ASP.NET Core app](xref:index) that runs on a web server.</span></span>

<span data-ttu-id="8509b-134">L'app webassembly Blazor client viene pubblicata nella cartella */bin/Release/{Target Framework}/Publish/wwwroot* dell'app Server, insieme a qualsiasi altra risorsa Web statica dell'app Server.</span><span class="sxs-lookup"><span data-stu-id="8509b-134">The client Blazor WebAssembly app is published into the */bin/Release/{TARGET FRAMEWORK}/publish/wwwroot* folder of the server app, along with any other static web assets of the server app.</span></span> <span data-ttu-id="8509b-135">Le due app vengono distribuite insieme.</span><span class="sxs-lookup"><span data-stu-id="8509b-135">The two apps are deployed together.</span></span> <span data-ttu-id="8509b-136">È necessario un server Web in grado di ospitare un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8509b-136">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="8509b-137">Per una distribuzione ospitata, Visual Studio include il modello di progetto **app webassemblyBlazor** (`blazorwasm` modello quando si usa il comando [DotNet New](/dotnet/core/tools/dotnet-new) ) con l'opzione **Hosted** selezionata (`-ho|--hosted` quando si usa il comando `dotnet new`).</span><span class="sxs-lookup"><span data-stu-id="8509b-137">For a hosted deployment, Visual Studio includes the **Blazor WebAssembly App** project template (`blazorwasm` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command) with the **Hosted** option selected (`-ho|--hosted` when using the `dotnet new` command).</span></span>

<span data-ttu-id="8509b-138">Per altre informazioni su hosting e distribuzione di app ASP.NET Core, vedere <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="8509b-138">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="8509b-139">Per informazioni sulla distribuzione in Servizio app di Azure, vedere <xref:tutorials/publish-to-azure-webapp-using-vs>.</span><span class="sxs-lookup"><span data-stu-id="8509b-139">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

## <a name="standalone-deployment"></a><span data-ttu-id="8509b-140">Distribuzione autonoma</span><span class="sxs-lookup"><span data-stu-id="8509b-140">Standalone deployment</span></span>

<span data-ttu-id="8509b-141">Una *distribuzione autonoma* serve l'app webassembly Blazor come un set di file statici richiesti direttamente dai client.</span><span class="sxs-lookup"><span data-stu-id="8509b-141">A *standalone deployment* serves the Blazor WebAssembly app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="8509b-142">Qualsiasi file server statico è in grado di gestire l'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="8509b-142">Any static file server is able to serve the Blazor app.</span></span>

<span data-ttu-id="8509b-143">Le risorse di distribuzione autonome vengono pubblicate nella cartella */bin/Release/{Target Framework}/Publish/wwwroot*</span><span class="sxs-lookup"><span data-stu-id="8509b-143">Standalone deployment assets are published into the */bin/Release/{TARGET FRAMEWORK}/publish/wwwroot* folder.</span></span>

### <a name="iis"></a><span data-ttu-id="8509b-144">IIS</span><span class="sxs-lookup"><span data-stu-id="8509b-144">IIS</span></span>

<span data-ttu-id="8509b-145">IIS è un file server statico idoneo per le app Blazor.</span><span class="sxs-lookup"><span data-stu-id="8509b-145">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="8509b-146">Per configurare IIS per l'hosting Blazor, vedere [la pagina relativa alla creazione di un sito Web statico in IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span><span class="sxs-lookup"><span data-stu-id="8509b-146">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="8509b-147">Gli asset pubblicati vengono creati nella cartella */bin/Release/{FRAMEWORK DESTINAZIONE}/publish*.</span><span class="sxs-lookup"><span data-stu-id="8509b-147">Published assets are created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="8509b-148">Ospitare il contenuto della cartella *publish* nel server Web o nel servizio di hosting.</span><span class="sxs-lookup"><span data-stu-id="8509b-148">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

#### <a name="webconfig"></a><span data-ttu-id="8509b-149">web.config</span><span class="sxs-lookup"><span data-stu-id="8509b-149">web.config</span></span>

<span data-ttu-id="8509b-150">Quando viene pubblicato un progetto di Blazor, viene creato un file *Web. config* con la seguente configurazione di IIS:</span><span class="sxs-lookup"><span data-stu-id="8509b-150">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="8509b-151">Vengono impostati tipi MIME per le estensioni di file seguenti:</span><span class="sxs-lookup"><span data-stu-id="8509b-151">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="8509b-152">*dll* &ndash; `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="8509b-152">*.dll* &ndash; `application/octet-stream`</span></span>
  * <span data-ttu-id="8509b-153">*json* &ndash; `application/json`</span><span class="sxs-lookup"><span data-stu-id="8509b-153">*.json* &ndash; `application/json`</span></span>
  * <span data-ttu-id="8509b-154">*. wasm* &ndash; `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="8509b-154">*.wasm* &ndash; `application/wasm`</span></span>
  * <span data-ttu-id="8509b-155">*. woff* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="8509b-155">*.woff* &ndash; `application/font-woff`</span></span>
  * <span data-ttu-id="8509b-156">*. woff2* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="8509b-156">*.woff2* &ndash; `application/font-woff`</span></span>
* <span data-ttu-id="8509b-157">Viene abilitata la compressione HTTP per i tipi MIME seguenti:</span><span class="sxs-lookup"><span data-stu-id="8509b-157">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="8509b-158">Vengono stabilite le regole URL Rewrite Module:</span><span class="sxs-lookup"><span data-stu-id="8509b-158">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="8509b-159">Specificare la sottodirectory in cui risiedono gli asset statici dell'app ( *{NOME ASSEMBLY}/dist/{PERCORSO RICHIESTO}* ).</span><span class="sxs-lookup"><span data-stu-id="8509b-159">Serve the sub-directory where the app's static assets reside (*{ASSEMBLY NAME}/dist/{PATH REQUESTED}*).</span></span>
  * <span data-ttu-id="8509b-160">Creare routing di fallback SPA in modo che le richieste per gli asset non file vengano reindirizzate al documento predefinito dell'app nella relativa cartella degli asset statici ( *{NOME ASSEMBLY}/dist/index.html*).</span><span class="sxs-lookup"><span data-stu-id="8509b-160">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*{ASSEMBLY NAME}/dist/index.html*).</span></span>

#### <a name="install-the-url-rewrite-module"></a><span data-ttu-id="8509b-161">Installare URL Rewrite Module</span><span class="sxs-lookup"><span data-stu-id="8509b-161">Install the URL Rewrite Module</span></span>

<span data-ttu-id="8509b-162">[URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) è necessario per riscrivere gli URL.</span><span class="sxs-lookup"><span data-stu-id="8509b-162">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="8509b-163">Il modulo non viene installato per impostazione predefinita e non è disponibile per l'installazione come funzionalità del servizio ruolo Server Web (IIS).</span><span class="sxs-lookup"><span data-stu-id="8509b-163">The module isn't installed by default, and it isn't available for install as a Web Server (IIS) role service feature.</span></span> <span data-ttu-id="8509b-164">Il modulo deve essere scaricato dal sito Web IIS.</span><span class="sxs-lookup"><span data-stu-id="8509b-164">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="8509b-165">Usare Installazione guidata piattaforma Web per installare il modulo:</span><span class="sxs-lookup"><span data-stu-id="8509b-165">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="8509b-166">In locale, passare alla [pagina di download di URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span><span class="sxs-lookup"><span data-stu-id="8509b-166">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="8509b-167">Per la versione in lingua inglese selezionare **WebPI** per scaricare il programma di installazione di Installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="8509b-167">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="8509b-168">Per altre lingue, selezionare l'architettura appropriata per il server (x86 o x64) per scaricare il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="8509b-168">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="8509b-169">Copiare il programma di installazione nel server.</span><span class="sxs-lookup"><span data-stu-id="8509b-169">Copy the installer to the server.</span></span> <span data-ttu-id="8509b-170">Eseguire il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="8509b-170">Run the installer.</span></span> <span data-ttu-id="8509b-171">Selezionare il pulsante **Installa** e accettare le condizioni di licenza.</span><span class="sxs-lookup"><span data-stu-id="8509b-171">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="8509b-172">Al termine dell'installazione non è necessario un riavvio del server.</span><span class="sxs-lookup"><span data-stu-id="8509b-172">A server restart isn't required after the install completes.</span></span>

#### <a name="configure-the-website"></a><span data-ttu-id="8509b-173">Configurare il sito Web</span><span class="sxs-lookup"><span data-stu-id="8509b-173">Configure the website</span></span>

<span data-ttu-id="8509b-174">Impostare il **percorso fisico** del sito Web sulla cartella dell'app.</span><span class="sxs-lookup"><span data-stu-id="8509b-174">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="8509b-175">La cartella contiene:</span><span class="sxs-lookup"><span data-stu-id="8509b-175">The folder contains:</span></span>

* <span data-ttu-id="8509b-176">Il file *web.config* usato da IIS per configurare il sito Web, inclusi le regole di reindirizzamento richieste e i tipi di contenuto di file.</span><span class="sxs-lookup"><span data-stu-id="8509b-176">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="8509b-177">Cartella degli asset statici dell'app.</span><span class="sxs-lookup"><span data-stu-id="8509b-177">The app's static asset folder.</span></span>

#### <a name="host-as-an-iis-sub-app"></a><span data-ttu-id="8509b-178">Ospitare come applicazione secondaria IIS</span><span class="sxs-lookup"><span data-stu-id="8509b-178">Host as an IIS sub-app</span></span>

<span data-ttu-id="8509b-179">Se un'app autonoma è ospitata come una sub-app IIS, eseguire una delle operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="8509b-179">If a standalone app is hosted as an IIS sub-app, perform either of the following:</span></span>

* <span data-ttu-id="8509b-180">Disabilitare il gestore del modulo ASP.NET Core ereditato.</span><span class="sxs-lookup"><span data-stu-id="8509b-180">Disable the inherited ASP.NET Core Module handler.</span></span>

  <span data-ttu-id="8509b-181">Rimuovere il gestore nel file *Web. config* pubblicato dell'app Blazor aggiungendo una `<handlers>` sezione al file:</span><span class="sxs-lookup"><span data-stu-id="8509b-181">Remove the handler in the Blazor app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>

  ```xml
  <handlers>
    <remove name="aspNetCore" />
  </handlers>
  ```

* <span data-ttu-id="8509b-182">Disabilitare l'ereditarietà della sezione `<system.webServer>` dell'app radice (padre) usando un elemento `<location>` con `inheritInChildApplications` impostato su `false`:</span><span class="sxs-lookup"><span data-stu-id="8509b-182">Disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <location path="." inheritInChildApplications="false">
      <system.webServer>
        <handlers>
          <add name="aspNetCore" ... />
        </handlers>
        <aspNetCore ... />
      </system.webServer>
    </location>
  </configuration>
  ```

<span data-ttu-id="8509b-183">La rimozione del gestore o la disabilitazione dell'ereditarietà viene eseguita oltre alla [configurazione del percorso di base dell'applicazione](xref:host-and-deploy/blazor/index#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="8509b-183">Removing the handler or disabling inheritance is performed in addition to [configuring the app's base path](xref:host-and-deploy/blazor/index#app-base-path).</span></span> <span data-ttu-id="8509b-184">Impostare il percorso di base dell'app nel file *index.html* dell'app sull'alias IIS usato durante la configurazione della sottoapp in IIS.</span><span class="sxs-lookup"><span data-stu-id="8509b-184">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

#### <a name="troubleshooting"></a><span data-ttu-id="8509b-185">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="8509b-185">Troubleshooting</span></span>

<span data-ttu-id="8509b-186">Se si riceve un *errore interno del server 500* e Gestione IIS genera errori durante il tentativo di accedere alla configurazione del sito Web, verificare che sia installato URL Rewrite Module.</span><span class="sxs-lookup"><span data-stu-id="8509b-186">If a *500 - Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="8509b-187">Quando il modulo non è installato, il file *web.config* non può essere analizzato da IIS.</span><span class="sxs-lookup"><span data-stu-id="8509b-187">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="8509b-188">In questo modo si impedisce al gestore IIS di caricare la configurazione del sito Web e il sito Web di di soddisfare i file statici di Blazor.</span><span class="sxs-lookup"><span data-stu-id="8509b-188">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="8509b-189">Per altre informazioni sulla risoluzione dei problemi relativi alle distribuzioni in IIS, vedere <xref:test/troubleshoot-azure-iis>.</span><span class="sxs-lookup"><span data-stu-id="8509b-189">For more information on troubleshooting deployments to IIS, see <xref:test/troubleshoot-azure-iis>.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="8509b-190">Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="8509b-190">Azure Storage</span></span>

<span data-ttu-id="8509b-191">L'hosting di file statici di [archiviazione di Azure](/azure/storage/) consente l'hosting di app Blazor senza server.</span><span class="sxs-lookup"><span data-stu-id="8509b-191">[Azure Storage](/azure/storage/) static file hosting allows serverless Blazor app hosting.</span></span> <span data-ttu-id="8509b-192">Sono supportati nomi di dominio personalizzati, la rete per la distribuzione di contenuti (rete CDN) di Azure e HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8509b-192">Custom domain names, the Azure Content Delivery Network (CDN), and HTTPS are supported.</span></span>

<span data-ttu-id="8509b-193">Quando il servizio BLOB è abilitato per l'hosting di siti Web statici in un account di archiviazione:</span><span class="sxs-lookup"><span data-stu-id="8509b-193">When the blob service is enabled for static website hosting on a storage account:</span></span>

* <span data-ttu-id="8509b-194">Impostare **Nome del documento di indice** su `index.html`.</span><span class="sxs-lookup"><span data-stu-id="8509b-194">Set the **Index document name** to `index.html`.</span></span>
* <span data-ttu-id="8509b-195">Impostare **Percorso del documento di errore** su `index.html`.</span><span class="sxs-lookup"><span data-stu-id="8509b-195">Set the **Error document path** to `index.html`.</span></span> <span data-ttu-id="8509b-196">I componenti di Razor e altri endpoint non basati su file non risiedono in percorsi fisici nel contenuto statico archiviato dal servizio BLOB.</span><span class="sxs-lookup"><span data-stu-id="8509b-196">Razor components and other non-file endpoints don't reside at physical paths in the static content stored by the blob service.</span></span> <span data-ttu-id="8509b-197">Quando viene ricevuta una richiesta per una di queste risorse che il router di Blazor deve gestire, l'errore *404 non trovato* generato dal servizio BLOB instrada la richiesta al percorso del **documento di errore**.</span><span class="sxs-lookup"><span data-stu-id="8509b-197">When a request for one of these resources is received that the Blazor router should handle, the *404 - Not Found* error generated by the blob service routes the request to the **Error document path**.</span></span> <span data-ttu-id="8509b-198">Viene restituito il BLOB *index. html* e il router Blazor carica ed elabora il percorso.</span><span class="sxs-lookup"><span data-stu-id="8509b-198">The *index.html* blob is returned, and the Blazor router loads and processes the path.</span></span>

<span data-ttu-id="8509b-199">Per altre informazioni, vedere [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website) (Hosting di siti Web statici in Archiviazione di Azure).</span><span class="sxs-lookup"><span data-stu-id="8509b-199">For more information, see [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span></span>

### <a name="nginx"></a><span data-ttu-id="8509b-200">Nginx</span><span class="sxs-lookup"><span data-stu-id="8509b-200">Nginx</span></span>

<span data-ttu-id="8509b-201">Il file *nginx.conf* seguente è semplificato per mostrare come configurare Nginx per l'invio del file *index.html* ogni volta che non riesce a trovare un file su disco corrispondente.</span><span class="sxs-lookup"><span data-stu-id="8509b-201">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *index.html* file whenever it can't find a corresponding file on disk.</span></span>

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /index.html =404;
        }
    }
}
```

<span data-ttu-id="8509b-202">Per altre informazioni sulla configurazione del server Web Nginx in ambiente di produzione, vedere [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/) (Creazione di file di configurazione NGINX Plus e NGINX).</span><span class="sxs-lookup"><span data-stu-id="8509b-202">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

### <a name="nginx-in-docker"></a><span data-ttu-id="8509b-203">Nginx in Docker</span><span class="sxs-lookup"><span data-stu-id="8509b-203">Nginx in Docker</span></span>

<span data-ttu-id="8509b-204">Per ospitare Blazor in Docker usando nginx, configurare Dockerfile per l'uso dell'immagine nginx basata su Alpine.</span><span class="sxs-lookup"><span data-stu-id="8509b-204">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="8509b-205">Aggiornare il Dockerfile per copiare il file *nginx.config* nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="8509b-205">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="8509b-206">Aggiungere una riga al Dockerfile, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="8509b-206">Add one line to the Dockerfile, as shown in the following example:</span></span>

```dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="apache"></a><span data-ttu-id="8509b-207">Apache</span><span class="sxs-lookup"><span data-stu-id="8509b-207">Apache</span></span>

<span data-ttu-id="8509b-208">Per distribuire un'app webassembly Blazor in CentOS 7 o versione successiva:</span><span class="sxs-lookup"><span data-stu-id="8509b-208">To deploy a Blazor WebAssembly app to CentOS 7 or later:</span></span>

1. <span data-ttu-id="8509b-209">Creare il file di configurazione Apache.</span><span class="sxs-lookup"><span data-stu-id="8509b-209">Create the Apache configuration file.</span></span> <span data-ttu-id="8509b-210">L'esempio seguente è un file di configurazione semplificato (*blazorapp. config*):</span><span class="sxs-lookup"><span data-stu-id="8509b-210">The following example is a simplified configuration file (*blazorapp.config*):</span></span>

   ```
   <VirtualHost *:80>
       ServerName www.example.com
       ServerAlias *.example.com

       DocumentRoot "/var/www/blazorapp"
       ErrorDocument 404 /index.html

       AddType application/wasm .wasm
       AddType application/octet-stream .dll
   
       <Directory "/var/www/blazorapp">
           Options -Indexes
           AllowOverride None
       </Directory>

       <IfModule mod_deflate.c>
           AddOutputFilterByType DEFLATE text/css
           AddOutputFilterByType DEFLATE application/javascript
           AddOutputFilterByType DEFLATE text/html
           AddOutputFilterByType DEFLATE application/octet-stream
           AddOutputFilterByType DEFLATE application/wasm
           <IfModule mod_setenvif.c>
           BrowserMatch ^Mozilla/4 gzip-only-text/html
           BrowserMatch ^Mozilla/4.0[678] no-gzip
           BrowserMatch bMSIE !no-gzip !gzip-only-text/html
       </IfModule>
       </IfModule>

       ErrorLog /var/log/httpd/blazorapp-error.log
       CustomLog /var/log/httpd/blazorapp-access.log common
   </VirtualHost>
   ```

1. <span data-ttu-id="8509b-211">Inserire il file di configurazione Apache nella directory `/etc/httpd/conf.d/`, ovvero la directory di configurazione predefinita di Apache in CentOS 7.</span><span class="sxs-lookup"><span data-stu-id="8509b-211">Place the Apache configuration file into the `/etc/httpd/conf.d/` directory, which is the default Apache configuration directory in CentOS 7.</span></span>

1. <span data-ttu-id="8509b-212">Inserire i file dell'app nella directory `/var/www/blazorapp` (il percorso specificato per `DocumentRoot` nel file di configurazione).</span><span class="sxs-lookup"><span data-stu-id="8509b-212">Place the app's files into the `/var/www/blazorapp` directory (the location specified to `DocumentRoot` in the configuration file).</span></span>

1. <span data-ttu-id="8509b-213">Riavviare il servizio Apache.</span><span class="sxs-lookup"><span data-stu-id="8509b-213">Restart the Apache service.</span></span>

<span data-ttu-id="8509b-214">Per ulteriori informazioni, vedere [mod_mime](https://httpd.apache.org/docs/2.4/mod/mod_mime.html) e [mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html).</span><span class="sxs-lookup"><span data-stu-id="8509b-214">For more information, see [mod_mime](https://httpd.apache.org/docs/2.4/mod/mod_mime.html) and [mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html).</span></span>

### <a name="github-pages"></a><span data-ttu-id="8509b-215">Pagine di GitHub</span><span class="sxs-lookup"><span data-stu-id="8509b-215">GitHub Pages</span></span>

<span data-ttu-id="8509b-216">Per gestire le riscritture degli URL, aggiungere un file *404.html* con uno script che gestisce il reindirizzamento della richiesta alla pagina *index.html*.</span><span class="sxs-lookup"><span data-stu-id="8509b-216">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="8509b-217">Per un esempio di implementazione fornito dalla community, vedere [Single Page Apps for GitHub Pages](https://spa-github-pages.rafrex.com/) (App a pagina singola per pagine GitHub) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span><span class="sxs-lookup"><span data-stu-id="8509b-217">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](https://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="8509b-218">È possibile visualizzare un esempio d'uso dell'approccio della community in [blazor-demo/blazor-demo.github.io su GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([sito live](https://blazor-demo.github.io/)).</span><span class="sxs-lookup"><span data-stu-id="8509b-218">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="8509b-219">Quando si usa un sito di progetto anziché un sito dell'organizzazione, aggiungere o aggiornare il tag `<base>` in *index.html*.</span><span class="sxs-lookup"><span data-stu-id="8509b-219">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="8509b-220">Impostare il valore dell'attributo `href` sul nome del repository GitHub con una barra finale (ad esempio, `my-repository/`.</span><span class="sxs-lookup"><span data-stu-id="8509b-220">Set the `href` attribute value to the GitHub repository name with a trailing slash (for example, `my-repository/`.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="8509b-221">Valori di configurazione dell'host</span><span class="sxs-lookup"><span data-stu-id="8509b-221">Host configuration values</span></span>

<span data-ttu-id="8509b-222">[Blazor le app webassembly](xref:blazor/hosting-models#blazor-webassembly) possono accettare i valori di configurazione host seguenti come argomenti della riga di comando in fase di esecuzione nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="8509b-222">[Blazor WebAssembly apps](xref:blazor/hosting-models#blazor-webassembly) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="8509b-223">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="8509b-223">Content root</span></span>

<span data-ttu-id="8509b-224">L'argomento `--contentroot` imposta il percorso assoluto della directory che contiene i file di contenuto dell'app ([radice del contenuto](xref:fundamentals/index#content-root)).</span><span class="sxs-lookup"><span data-stu-id="8509b-224">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files ([content root](xref:fundamentals/index#content-root)).</span></span> <span data-ttu-id="8509b-225">Negli esempi seguenti `/content-root-path` è il percorso radice del contenuto dell'app.</span><span class="sxs-lookup"><span data-stu-id="8509b-225">In the following examples, `/content-root-path` is the app's content root path.</span></span>

* <span data-ttu-id="8509b-226">Passare l'argomento quando si esegue localmente l'app a un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="8509b-226">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="8509b-227">Dalla directory dell'app, eseguire:</span><span class="sxs-lookup"><span data-stu-id="8509b-227">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --contentroot=/content-root-path
  ```

* <span data-ttu-id="8509b-228">Aggiungere una voce al file *launchSettings.json* dell'app nel profilo **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="8509b-228">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="8509b-229">Questa impostazione viene usata quando l'app viene eseguita con il debugger di Visual Studio e da un prompt dei comandi con `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="8509b-229">This setting is used when the app is run with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* <span data-ttu-id="8509b-230">In Visual Studio specificare l'argomento in **Proprietà** > **Debug** > **Argomenti applicazione**.</span><span class="sxs-lookup"><span data-stu-id="8509b-230">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="8509b-231">Impostando l'argomento nella pagina delle proprietà di Visual Studio, si aggiunge l'argomento al file *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="8509b-231">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a><span data-ttu-id="8509b-232">Base del percorso</span><span class="sxs-lookup"><span data-stu-id="8509b-232">Path base</span></span>

<span data-ttu-id="8509b-233">L'argomento `--pathbase` imposta il percorso di base dell'app per un'app eseguita localmente con un percorso URL relativo non radice (il tag `<base>` `href` è impostato su un percorso diverso da `/` per la gestione temporanea e la produzione).</span><span class="sxs-lookup"><span data-stu-id="8509b-233">The `--pathbase` argument sets the app base path for an app run locally with a non-root relative URL path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="8509b-234">Negli esempi seguenti `/relative-URL-path` è la base del percorso dell'app.</span><span class="sxs-lookup"><span data-stu-id="8509b-234">In the following examples, `/relative-URL-path` is the app's path base.</span></span> <span data-ttu-id="8509b-235">Per altre informazioni, vedere [percorso di base dell'app](xref:host-and-deploy/blazor/index#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="8509b-235">For more information, see [App base path](xref:host-and-deploy/blazor/index#app-base-path).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8509b-236">A differenza del percorso specificato per `href` del tag `<base>`, non includere una barra finale (`/`) quando si passa il valore dell'argomento `--pathbase`.</span><span class="sxs-lookup"><span data-stu-id="8509b-236">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="8509b-237">Se il percorso di base dell'app non viene specificato nel tag `<base>` come `<base href="/CoolApp/">` (include una barra finale), passare il valore dell'argomento della riga di comando come `--pathbase=/CoolApp` (senza barra finale).</span><span class="sxs-lookup"><span data-stu-id="8509b-237">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/">` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="8509b-238">Passare l'argomento quando si esegue localmente l'app a un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="8509b-238">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="8509b-239">Dalla directory dell'app, eseguire:</span><span class="sxs-lookup"><span data-stu-id="8509b-239">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --pathbase=/relative-URL-path
  ```

* <span data-ttu-id="8509b-240">Aggiungere una voce al file *launchSettings.json* dell'app nel profilo **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="8509b-240">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="8509b-241">Questa impostazione viene usata quando l'app viene eseguita con il debugger di Visual Studio e da un prompt dei comandi con `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="8509b-241">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/relative-URL-path"
  ```

* <span data-ttu-id="8509b-242">In Visual Studio specificare l'argomento in **Proprietà** > **Debug** > **Argomenti applicazione**.</span><span class="sxs-lookup"><span data-stu-id="8509b-242">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="8509b-243">Impostando l'argomento nella pagina delle proprietà di Visual Studio, si aggiunge l'argomento al file *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="8509b-243">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/relative-URL-path
  ```

### <a name="urls"></a><span data-ttu-id="8509b-244">URL</span><span class="sxs-lookup"><span data-stu-id="8509b-244">URLs</span></span>

<span data-ttu-id="8509b-245">L'argomento `--urls` imposta gli indirizzi IP o gli indirizzi host con le porte e i protocolli su cui eseguire l'ascolto per le richieste.</span><span class="sxs-lookup"><span data-stu-id="8509b-245">The `--urls` argument sets the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="8509b-246">Passare l'argomento quando si esegue localmente l'app a un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="8509b-246">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="8509b-247">Dalla directory dell'app, eseguire:</span><span class="sxs-lookup"><span data-stu-id="8509b-247">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="8509b-248">Aggiungere una voce al file *launchSettings.json* dell'app nel profilo **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="8509b-248">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="8509b-249">Questa impostazione viene usata quando l'app viene eseguita con il debugger di Visual Studio e da un prompt dei comandi con `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="8509b-249">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="8509b-250">In Visual Studio specificare l'argomento in **Proprietà** > **Debug** > **Argomenti applicazione**.</span><span class="sxs-lookup"><span data-stu-id="8509b-250">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="8509b-251">Impostando l'argomento nella pagina delle proprietà di Visual Studio, si aggiunge l'argomento al file *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="8509b-251">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="configure-the-linker"></a><span data-ttu-id="8509b-252">Configurare il linker</span><span class="sxs-lookup"><span data-stu-id="8509b-252">Configure the Linker</span></span>

Blazor<span data-ttu-id="8509b-253"> esegue il collegamento Intermediate Language (IL) a ogni build di rilascio per rimuovere IL linguaggio IL non necessario dagli assembly di output.</span><span class="sxs-lookup"><span data-stu-id="8509b-253"> performs Intermediate Language (IL) linking on each Release build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="8509b-254">Per altre informazioni, vedere <xref:host-and-deploy/blazor/configure-linker>.</span><span class="sxs-lookup"><span data-stu-id="8509b-254">For more information, see <xref:host-and-deploy/blazor/configure-linker>.</span></span>
