---
title: Ospitare e distribuire Blazor sul lato client
author: guardrex
description: Informazioni su come ospitare e distribuire un'app Blazor con ASP.NET Core, reti per la distribuzione di contenuti (CDN), file server e pagine di GitHub.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/21/2019
uid: host-and-deploy/blazor/client-side
ms.openlocfilehash: b50516b4dce28a6b105b2ab8b9386060d5392983
ms.sourcegitcommit: 4d05e30567279072f1b070618afe58ae1bcefd5a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376398"
---
# <a name="host-and-deploy-blazor-client-side"></a><span data-ttu-id="9f95c-103">Ospitare e distribuire Blazor sul lato client</span><span class="sxs-lookup"><span data-stu-id="9f95c-103">Host and deploy Blazor client-side</span></span>

<span data-ttu-id="9f95c-104">Di [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="9f95c-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="9f95c-105">Valori di configurazione dell'host</span><span class="sxs-lookup"><span data-stu-id="9f95c-105">Host configuration values</span></span>

<span data-ttu-id="9f95c-106">Le app Blazor che usano il [modello di hosting sul lato client](xref:blazor/hosting-models#client-side) possono accettare i valori di configurazione dell'host seguenti come argomenti della riga di comando in fase di esecuzione nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="9f95c-106">Blazor apps that use the [client-side hosting model](xref:blazor/hosting-models#client-side) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="9f95c-107">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="9f95c-107">Content Root</span></span>

<span data-ttu-id="9f95c-108">L'argomento `--contentroot` imposta il percorso assoluto sulla directory che contiene i file di contenuto dell'app.</span><span class="sxs-lookup"><span data-stu-id="9f95c-108">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files.</span></span> <span data-ttu-id="9f95c-109">Negli esempi seguenti `/content-root-path` è il percorso radice del contenuto dell'app.</span><span class="sxs-lookup"><span data-stu-id="9f95c-109">In the following examples, `/content-root-path` is the app's content root path.</span></span>

* <span data-ttu-id="9f95c-110">Passare l'argomento quando si esegue localmente l'app a un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="9f95c-110">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="9f95c-111">Dalla directory dell'app, eseguire:</span><span class="sxs-lookup"><span data-stu-id="9f95c-111">From the app's directory, execute:</span></span>

  ```console
  dotnet run --contentroot=/content-root-path
  ```

* <span data-ttu-id="9f95c-112">Aggiungere una voce al file *launchSettings.json* dell'app nel profilo **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="9f95c-112">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="9f95c-113">Questa impostazione viene usata quando l'app viene eseguita con il debugger di Visual Studio e da un prompt dei comandi con `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="9f95c-113">This setting is used when the app is run with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* <span data-ttu-id="9f95c-114">In Visual Studio specificare l'argomento in **Proprietà** > **Debug** > **Argomenti applicazione**.</span><span class="sxs-lookup"><span data-stu-id="9f95c-114">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="9f95c-115">Impostando l'argomento nella pagina delle proprietà di Visual Studio, si aggiunge l'argomento al file *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9f95c-115">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a><span data-ttu-id="9f95c-116">Base del percorso</span><span class="sxs-lookup"><span data-stu-id="9f95c-116">Path base</span></span>

<span data-ttu-id="9f95c-117">L'argomento `--pathbase` imposta il percorso di base dell'app per un'app eseguita in locale con un percorso virtuale non radice (il tag `<base>` `href` è impostato su un percorso diverso da `/` per staging e produzione).</span><span class="sxs-lookup"><span data-stu-id="9f95c-117">The `--pathbase` argument sets the app base path for an app run locally with a non-root virtual path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="9f95c-118">Negli esempi seguenti `/virtual-path` è la base del percorso dell'app.</span><span class="sxs-lookup"><span data-stu-id="9f95c-118">In the following examples, `/virtual-path` is the app's path base.</span></span> <span data-ttu-id="9f95c-119">Per altre informazioni, vedere la sezione [Percorso di base dell'app](#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="9f95c-119">For more information, see the [App base path](#app-base-path) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9f95c-120">A differenza del percorso specificato per `href` del tag `<base>`, non includere una barra finale (`/`) quando si passa il valore dell'argomento `--pathbase`.</span><span class="sxs-lookup"><span data-stu-id="9f95c-120">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="9f95c-121">Se il percorso di base dell'app non viene specificato nel tag `<base>` come `<base href="/CoolApp/">` (include una barra finale), passare il valore dell'argomento della riga di comando come `--pathbase=/CoolApp` (senza barra finale).</span><span class="sxs-lookup"><span data-stu-id="9f95c-121">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/">` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="9f95c-122">Passare l'argomento quando si esegue localmente l'app a un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="9f95c-122">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="9f95c-123">Dalla directory dell'app, eseguire:</span><span class="sxs-lookup"><span data-stu-id="9f95c-123">From the app's directory, execute:</span></span>

  ```console
  dotnet run --pathbase=/virtual-path
  ```

* <span data-ttu-id="9f95c-124">Aggiungere una voce al file *launchSettings.json* dell'app nel profilo **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="9f95c-124">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="9f95c-125">Questa impostazione viene usata quando l'app viene eseguita con il debugger di Visual Studio e da un prompt dei comandi con `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="9f95c-125">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/virtual-path"
  ```

* <span data-ttu-id="9f95c-126">In Visual Studio specificare l'argomento in **Proprietà** > **Debug** > **Argomenti applicazione**.</span><span class="sxs-lookup"><span data-stu-id="9f95c-126">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="9f95c-127">Impostando l'argomento nella pagina delle proprietà di Visual Studio, si aggiunge l'argomento al file *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9f95c-127">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/virtual-path
  ```

### <a name="urls"></a><span data-ttu-id="9f95c-128">URL</span><span class="sxs-lookup"><span data-stu-id="9f95c-128">URLs</span></span>

<span data-ttu-id="9f95c-129">L'argomento `--urls` imposta gli indirizzi IP o gli indirizzi host con le porte e i protocolli su cui eseguire l'ascolto per le richieste.</span><span class="sxs-lookup"><span data-stu-id="9f95c-129">The `--urls` argument sets the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="9f95c-130">Passare l'argomento quando si esegue localmente l'app a un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="9f95c-130">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="9f95c-131">Dalla directory dell'app, eseguire:</span><span class="sxs-lookup"><span data-stu-id="9f95c-131">From the app's directory, execute:</span></span>

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="9f95c-132">Aggiungere una voce al file *launchSettings.json* dell'app nel profilo **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="9f95c-132">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="9f95c-133">Questa impostazione viene usata quando l'app viene eseguita con il debugger di Visual Studio e da un prompt dei comandi con `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="9f95c-133">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="9f95c-134">In Visual Studio specificare l'argomento in **Proprietà** > **Debug** > **Argomenti applicazione**.</span><span class="sxs-lookup"><span data-stu-id="9f95c-134">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="9f95c-135">Impostando l'argomento nella pagina delle proprietà di Visual Studio, si aggiunge l'argomento al file *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9f95c-135">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deployment"></a><span data-ttu-id="9f95c-136">Distribuzione</span><span class="sxs-lookup"><span data-stu-id="9f95c-136">Deployment</span></span>

<span data-ttu-id="9f95c-137">Con il [modello di hosting sul lato client](xref:blazor/hosting-models#client-side):</span><span class="sxs-lookup"><span data-stu-id="9f95c-137">With the [client-side hosting model](xref:blazor/hosting-models#client-side):</span></span>

* <span data-ttu-id="9f95c-138">L'app Blazor, le relative dipendenze e il runtime .NET vengono scaricati nel browser.</span><span class="sxs-lookup"><span data-stu-id="9f95c-138">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="9f95c-139">L'app viene eseguita direttamente nel thread dell'interfaccia utente del browser.</span><span class="sxs-lookup"><span data-stu-id="9f95c-139">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="9f95c-140">È supportata una delle strategie seguenti:</span><span class="sxs-lookup"><span data-stu-id="9f95c-140">Either of the following strategies is supported:</span></span>
  * <span data-ttu-id="9f95c-141">L'app Blazor viene fornita da un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9f95c-141">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="9f95c-142">Questa strategia viene trattata nella sezione [Distribuzione ospitata con ASP.NET Core](#hosted-deployment-with-aspnet-core).</span><span class="sxs-lookup"><span data-stu-id="9f95c-142">This strategy is covered in the [Hosted deployment with ASP.NET Core](#hosted-deployment-with-aspnet-core) section.</span></span>
  * <span data-ttu-id="9f95c-143">L'app Blazor viene posizionata in un server o un servizio Web di hosting statico, in cui non viene usato .NET per fornire l'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="9f95c-143">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="9f95c-144">Questa strategia viene trattata nella sezione [Distribuzione autonoma](#standalone-deployment).</span><span class="sxs-lookup"><span data-stu-id="9f95c-144">This strategy is covered in the [Standalone deployment](#standalone-deployment) section.</span></span>

## <a name="configure-the-linker"></a><span data-ttu-id="9f95c-145">Configurare il linker</span><span class="sxs-lookup"><span data-stu-id="9f95c-145">Configure the Linker</span></span>

<span data-ttu-id="9f95c-146">Blazor esegue il collegamento di linguaggio intermedio (IL) in ogni build per rimuovere IL non necessario dagli assembly di output.</span><span class="sxs-lookup"><span data-stu-id="9f95c-146">Blazor performs Intermediate Language (IL) linking on each build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="9f95c-147">Il collegamento degli assembly può essere controllato in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="9f95c-147">Assembly linking can be controlled on build.</span></span> <span data-ttu-id="9f95c-148">Per ulteriori informazioni, vedere <xref:host-and-deploy/blazor/configure-linker>.</span><span class="sxs-lookup"><span data-stu-id="9f95c-148">For more information, see <xref:host-and-deploy/blazor/configure-linker>.</span></span>

## <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="9f95c-149">Riscrivere gli URL per il routing corretto</span><span class="sxs-lookup"><span data-stu-id="9f95c-149">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="9f95c-150">Il routing delle richieste per i componenti di pagina in un'app sul lato client non è semplice come il routing delle richieste a un'app ospitata sul lato server.</span><span class="sxs-lookup"><span data-stu-id="9f95c-150">Routing requests for page components in a client-side app isn't as simple as routing requests to a server-side, hosted app.</span></span> <span data-ttu-id="9f95c-151">Si consideri un'app sul lato client con due pagine:</span><span class="sxs-lookup"><span data-stu-id="9f95c-151">Consider a client-side app with two pages:</span></span>

* <span data-ttu-id="9f95c-152">**_Main.razor**&ndash; Viene caricata nella radice dell'app e contiene un collegamento alla pagina About (`href="About"`).</span><span class="sxs-lookup"><span data-stu-id="9f95c-152">**_Main.razor** &ndash; Loads at the root of the app and contains a link to the About page (`href="About"`).</span></span>
* <span data-ttu-id="9f95c-153">**_About.Razor** &ndash; Pagina About.</span><span class="sxs-lookup"><span data-stu-id="9f95c-153">**_About.razor** &ndash; About page.</span></span>

<span data-ttu-id="9f95c-154">Quando viene richiesto il documento predefinito dell'app usando la barra degli indirizzi del browser (ad esempio, `https://www.contoso.com/`):</span><span class="sxs-lookup"><span data-stu-id="9f95c-154">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="9f95c-155">Il browser invia una richiesta.</span><span class="sxs-lookup"><span data-stu-id="9f95c-155">The browser makes a request.</span></span>
1. <span data-ttu-id="9f95c-156">Viene restituita la pagina predefinita, di solito *index.html*.</span><span class="sxs-lookup"><span data-stu-id="9f95c-156">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="9f95c-157">*index.html* esegue il bootstrap dell'app.</span><span class="sxs-lookup"><span data-stu-id="9f95c-157">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="9f95c-158">Il router di Blazor viene caricato e viene visualizzata la pagina principale di Razor (*Main.razor*).</span><span class="sxs-lookup"><span data-stu-id="9f95c-158">Blazor's router loads, and the Razor Main page (*Main.razor*) is displayed.</span></span>

<span data-ttu-id="9f95c-159">Nella pagina principale, se si seleziona il collegamento alla pagina About viene caricata tale pagina.</span><span class="sxs-lookup"><span data-stu-id="9f95c-159">On the Main page, selecting the link to the About page loads the About page.</span></span> <span data-ttu-id="9f95c-160">La selezione del collegamento alla pagina About funziona nel client perché il router Blazor impedisce al browser di inviare una richiesta su Internet a `www.contoso.com` per `About` e fornisce la pagina About autonomamente.</span><span class="sxs-lookup"><span data-stu-id="9f95c-160">Selecting the link to the About page works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the About page itself.</span></span> <span data-ttu-id="9f95c-161">Tutte le richieste di pagine interne *all'interno dell'app sul lato client* funzionano allo stesso modo: Le richieste non attivano richieste basate su browser a risorse ospitate nel server su Internet.</span><span class="sxs-lookup"><span data-stu-id="9f95c-161">All of the requests for internal pages *within the client-side app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="9f95c-162">Le richieste vengono gestite internamente dal router.</span><span class="sxs-lookup"><span data-stu-id="9f95c-162">The router handles the requests internally.</span></span>

<span data-ttu-id="9f95c-163">Se una richiesta viene effettuata usando la barra degli indirizzi del browser per `www.contoso.com/About`, la richiesta ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="9f95c-163">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="9f95c-164">La risorsa non esiste nell'host Internet dell'app, quindi viene restituita la risposta *404 non trovato*.</span><span class="sxs-lookup"><span data-stu-id="9f95c-164">No such resource exists on the app's Internet host, so a *404 - Not Found* response is returned.</span></span>

<span data-ttu-id="9f95c-165">Dato che i browser inviano le richieste agli host basati su Internet per le pagine sul lato client, i server Web e i servizi di hosting devono riscrivere tutte le richieste per le risorse che non si trovano fisicamente nel server per la pagina *index.html*.</span><span class="sxs-lookup"><span data-stu-id="9f95c-165">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="9f95c-166">Quando viene restituita la pagina *index.html*, il router sul lato client dell'app subentra e risponde con la risorsa corretta.</span><span class="sxs-lookup"><span data-stu-id="9f95c-166">When *index.html* is returned, the app's client-side router takes over and responds with the correct resource.</span></span>

## <a name="app-base-path"></a><span data-ttu-id="9f95c-167">Percorso di base dell'app</span><span class="sxs-lookup"><span data-stu-id="9f95c-167">App base path</span></span>

<span data-ttu-id="9f95c-168">Il *percorso di base dell'app* è il percorso radice dell'app virtuale nel server.</span><span class="sxs-lookup"><span data-stu-id="9f95c-168">The *app base path* is the virtual app root path on the server.</span></span> <span data-ttu-id="9f95c-169">Ad esempio, un'app che si trova nel server di Contoso in una cartella virtuale in `/CoolApp/` è raggiungibile all'indirizzo `https://www.contoso.com/CoolApp` e ha un percorso virtuale di base `/CoolApp/`.</span><span class="sxs-lookup"><span data-stu-id="9f95c-169">For example, an app that resides on the Contoso server in a virtual folder at `/CoolApp/` is reached at `https://www.contoso.com/CoolApp` and has a virtual base path of `/CoolApp/`.</span></span> <span data-ttu-id="9f95c-170">Impostando il percorso di base dell'app sul percorso virtuale (`<base href="/CoolApp/">`), l'app viene informata della posizione in cui risiede virtualmente nel server.</span><span class="sxs-lookup"><span data-stu-id="9f95c-170">By setting the app base path to the virtual path (`<base href="/CoolApp/">`), the app is made aware of where it virtually resides on the server.</span></span> <span data-ttu-id="9f95c-171">L'app può usare il percorso di base dell'app per costruire URL relativi rispetto alla radice dell'app da un componente che non si trova nella directory radice.</span><span class="sxs-lookup"><span data-stu-id="9f95c-171">The app can use the app base path to construct URLs relative to the app root from a component that isn't in the root directory.</span></span> <span data-ttu-id="9f95c-172">In questo modo i componenti presenti a diversi livelli della struttura di directory possono creare collegamenti ad altre risorse in varie posizioni in tutta l'app.</span><span class="sxs-lookup"><span data-stu-id="9f95c-172">This allows components that exist at different levels of the directory structure to build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="9f95c-173">Il percorso di base dell'app viene anche usato per intercettare i clic su collegamenti ipertestuali in cui la destinazione del collegamento `href` si trova all'interno dello spazio URI del percorso di base dell'app. Il router Blazor gestisce la navigazione interna.</span><span class="sxs-lookup"><span data-stu-id="9f95c-173">The app base path is also used to intercept hyperlink clicks where the `href` target of the link is within the app base path URI space&mdash;the Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="9f95c-174">In molti scenari di hosting, il percorso virtuale del server per l'app è la radice dell'app.</span><span class="sxs-lookup"><span data-stu-id="9f95c-174">In many hosting scenarios, the server's virtual path to the app is the root of the app.</span></span> <span data-ttu-id="9f95c-175">In questi casi, il percorso di base dell'app è una barra (`<base href="/" />`), ovvero la configurazione predefinita per un'app.</span><span class="sxs-lookup"><span data-stu-id="9f95c-175">In these cases, the app base path is a forward slash (`<base href="/" />`), which is the default configuration for an app.</span></span> <span data-ttu-id="9f95c-176">In altri scenari di hosting, ad esempio le pagine di GitHub, le directory virtuali di IIS e o le sottoapplicazioni, è necessario impostare il percorso di base dell'app sul percorso virtuale del server per l'app.</span><span class="sxs-lookup"><span data-stu-id="9f95c-176">In other hosting scenarios, such as GitHub Pages and IIS virtual directories or sub-applications, the app base path must be set to the server's virtual path to the app.</span></span> <span data-ttu-id="9f95c-177">Per impostare il percorso di base dell'app, aggiornare il tag `<base>` all'interno degli elementi del tag `<head>` del file*wwwroot/index.html*.</span><span class="sxs-lookup"><span data-stu-id="9f95c-177">To set the app's base path, update the `<base>` tag within the `<head>` tag elements of the *wwwroot/index.html* file.</span></span> <span data-ttu-id="9f95c-178">Impostare il valore dell'attributo `href` su `/virtual-path/` (la barra finale è obbligatoria), dove `/virtual-path/` è il percorso radice dell'app virtuale completo nel server per l'app.</span><span class="sxs-lookup"><span data-stu-id="9f95c-178">Set the `href` attribute value to `/virtual-path/` (the trailing slash is required), where `/virtual-path/` is the full virtual app root path on the server for the app.</span></span> <span data-ttu-id="9f95c-179">Nell'esempio precedente, il percorso virtuale è impostato su `/CoolApp/`: `<base href="/CoolApp/">`.</span><span class="sxs-lookup"><span data-stu-id="9f95c-179">In the preceding example, the virtual path is set to `/CoolApp/`: `<base href="/CoolApp/">`.</span></span>

<span data-ttu-id="9f95c-180">Per un'app con un percorso virtuale non radice configurato (ad esempio, `<base href="/CoolApp/">`), l'app non riesce a trovare le relative risorse *quando viene eseguita in locale*.</span><span class="sxs-lookup"><span data-stu-id="9f95c-180">For an app with a non-root virtual path configured (for example, `<base href="/CoolApp/">`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="9f95c-181">Per risolvere questo problema durante sviluppo e test locali, è possibile specificare un argomento *base del percorso* corrispondente al valore `href` del tag `<base>` in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="9f95c-181">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span>

<span data-ttu-id="9f95c-182">Per passare l'argomento di base del percorso con il percorso radice (`/`) quando si esegue l'app in locale, eseguire il comando `dotnet run` dalla directory dell'app usando l'opzione `--pathbase`:</span><span class="sxs-lookup"><span data-stu-id="9f95c-182">To pass the path base argument with the root path (`/`) when running the app locally, execute the `dotnet run` command from the app's directory with the `--pathbase` option:</span></span>

```console
dotnet run --pathbase=/{Virtual Path (no trailing slash)}
```

<span data-ttu-id="9f95c-183">Per un'app con un percorso virtuale di base di `/CoolApp/` (`<base href="/CoolApp/">`), il comando è:</span><span class="sxs-lookup"><span data-stu-id="9f95c-183">For an app with a virtual base path of `/CoolApp/` (`<base href="/CoolApp/">`), the command is:</span></span>

```console
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="9f95c-184">L'app risponde in locale all'indirizzo `http://localhost:port/CoolApp`.</span><span class="sxs-lookup"><span data-stu-id="9f95c-184">The app responds locally at `http://localhost:port/CoolApp`.</span></span>

<span data-ttu-id="9f95c-185">Per altre informazioni, vedere la sezione sul [valore di configurazione dell'host della base del percorso](#path-base).</span><span class="sxs-lookup"><span data-stu-id="9f95c-185">For more information, see the section on the [path base host configuration value](#path-base).</span></span>

<span data-ttu-id="9f95c-186">Se un'app usa il [modello di hosting sul lato client](xref:blazor/hosting-models#client-side) (basato sul modello di progetto **Blazor**, il modello `blazor` quando si usa il comando [dotnet new](/dotnet/core/tools/dotnet-new)) ed è ospitata come sottoapplicazione IIS in un'app ASP.NET Core, è importante disabilitare il gestore del modulo ASP.NET Core ereditato o assicurarsi che la sezione `<handlers>` dell'app radice (padre) nel file *web.config* non venga ereditata dalla sottoapplicazione.</span><span class="sxs-lookup"><span data-stu-id="9f95c-186">If an app uses the [client-side hosting model](xref:blazor/hosting-models#client-side) (based on the **Blazor** project template; the `blazor` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command) and is hosted as an IIS sub-application in an ASP.NET Core app, it's important to disable the inherited ASP.NET Core Module handler or make sure the root (parent) app's `<handlers>` section in the *web.config* file isn't inherited by the sub-app.</span></span>

<span data-ttu-id="9f95c-187">Rimuovere il gestore nel file *web.config* pubblicato dell'app aggiungendo una sezione `<handlers>` al file:</span><span class="sxs-lookup"><span data-stu-id="9f95c-187">Remove the handler in the app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>

```xml
<handlers>
  <remove name="aspNetCore" />
</handlers>
```

<span data-ttu-id="9f95c-188">In alternativa, disabilitare l'ereditarietà della sezione `<system.webServer>` dell'app radice (padre) usando un elemento `<location>` con `inheritInChildApplications` impostato su `false`:</span><span class="sxs-lookup"><span data-stu-id="9f95c-188">Alternatively, disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>

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

<span data-ttu-id="9f95c-189">La rimozione del gestore o la disabilitazione dell'ereditarietà viene eseguita in aggiunta alla configurazione del percorso di base dell'app come descritto in questa sezione.</span><span class="sxs-lookup"><span data-stu-id="9f95c-189">Removing the handler or disabling inheritance is performed in addition to configuring the app's base path as described in this section.</span></span> <span data-ttu-id="9f95c-190">Impostare il percorso di base dell'app nel file *index.html* dell'app sull'alias IIS usato durante la configurazione della sottoapp in IIS.</span><span class="sxs-lookup"><span data-stu-id="9f95c-190">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

## <a name="hosted-deployment-with-aspnet-core"></a><span data-ttu-id="9f95c-191">Distribuzione ospitata con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9f95c-191">Hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="9f95c-192">Una *distribuzione ospitata* fornisce l'app Blazor sul lato client ai browser da un'[app ASP.NET Core](xref:index) eseguita in un server.</span><span class="sxs-lookup"><span data-stu-id="9f95c-192">A *hosted deployment* serves the client-side Blazor app to browsers from an [ASP.NET Core app](xref:index) that runs on a server.</span></span>

<span data-ttu-id="9f95c-193">L'app Blazor è inclusa con l'app ASP.NET Core nell'output pubblicato in modo che le due app vengano distribuite insieme.</span><span class="sxs-lookup"><span data-stu-id="9f95c-193">The Blazor app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="9f95c-194">È necessario un server Web in grado di ospitare un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9f95c-194">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="9f95c-195">Per una distribuzione ospitata, Visual Studio include il modello di progetto **Blazor (ospitato in ASP.NET Core)** (modello `blazorhosted` quando si usa il comando [dotnet new](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="9f95c-195">For a hosted deployment, Visual Studio includes the **Blazor (ASP.NET Core hosted)** project template (`blazorhosted` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<span data-ttu-id="9f95c-196">Per altre informazioni su hosting e distribuzione di app ASP.NET Core, vedere <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="9f95c-196">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="9f95c-197">Per informazioni sulla distribuzione in Servizio app di Azure, vedere <xref:tutorials/publish-to-azure-webapp-using-vs>.</span><span class="sxs-lookup"><span data-stu-id="9f95c-197">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

## <a name="standalone-deployment"></a><span data-ttu-id="9f95c-198">Distribuzione autonoma</span><span class="sxs-lookup"><span data-stu-id="9f95c-198">Standalone deployment</span></span>

<span data-ttu-id="9f95c-199">Una *distribuzione autonoma* serve l'app Blazor sul lato client come un set di file statici richiesti direttamente dai client.</span><span class="sxs-lookup"><span data-stu-id="9f95c-199">A *standalone deployment* serves the client-side Blazor app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="9f95c-200">Qualsiasi file server statico è in grado di servire l'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="9f95c-200">Any static file server is able to serve the Blazor app.</span></span>

<span data-ttu-id="9f95c-201">Gli asset di distribuzione autonoma vengono pubblicati nella cartella */bin/Release/{FRAMEWORK DI DESTINAZIONE}/publish/{NOME ASSEMBLY}/dist*.</span><span class="sxs-lookup"><span data-stu-id="9f95c-201">Standalone deployment assets are published to the *bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist* folder.</span></span>

### <a name="iis"></a><span data-ttu-id="9f95c-202">IIS</span><span class="sxs-lookup"><span data-stu-id="9f95c-202">IIS</span></span>

<span data-ttu-id="9f95c-203">IIS è un file server statico per app Blazor.</span><span class="sxs-lookup"><span data-stu-id="9f95c-203">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="9f95c-204">Per configurare IIS per ospitare Blazor, vedere [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis) (Creare un sito Web statico in IIS).</span><span class="sxs-lookup"><span data-stu-id="9f95c-204">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="9f95c-205">Gli asset pubblicati vengono creati nella cartella */bin/Release/{FRAMEWORK DESTINAZIONE}/publish*.</span><span class="sxs-lookup"><span data-stu-id="9f95c-205">Published assets are created in the */bin/Release/{TARGET FRAMEWORK}/publish* folder.</span></span> <span data-ttu-id="9f95c-206">Ospitare il contenuto della cartella *publish* nel server Web o nel servizio di hosting.</span><span class="sxs-lookup"><span data-stu-id="9f95c-206">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

#### <a name="webconfig"></a><span data-ttu-id="9f95c-207">web.config</span><span class="sxs-lookup"><span data-stu-id="9f95c-207">web.config</span></span>

<span data-ttu-id="9f95c-208">Quando viene pubblicato un progetto Blazor, viene creato un file *web.config* con la configurazione di IIS seguente:</span><span class="sxs-lookup"><span data-stu-id="9f95c-208">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="9f95c-209">Vengono impostati tipi MIME per le estensioni di file seguenti:</span><span class="sxs-lookup"><span data-stu-id="9f95c-209">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="9f95c-210">*.dll* &ndash; `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="9f95c-210">*.dll* &ndash; `application/octet-stream`</span></span>
  * <span data-ttu-id="9f95c-211">*.json* &ndash; `application/json`</span><span class="sxs-lookup"><span data-stu-id="9f95c-211">*.json* &ndash; `application/json`</span></span>
  * <span data-ttu-id="9f95c-212">*.wasm* &ndash; `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="9f95c-212">*.wasm* &ndash; `application/wasm`</span></span>
  * <span data-ttu-id="9f95c-213">*.woff* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="9f95c-213">*.woff* &ndash; `application/font-woff`</span></span>
  * <span data-ttu-id="9f95c-214">*.woff2* &ndash; `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="9f95c-214">*.woff2* &ndash; `application/font-woff`</span></span>
* <span data-ttu-id="9f95c-215">Viene abilitata la compressione HTTP per i tipi MIME seguenti:</span><span class="sxs-lookup"><span data-stu-id="9f95c-215">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="9f95c-216">Vengono stabilite le regole URL Rewrite Module:</span><span class="sxs-lookup"><span data-stu-id="9f95c-216">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="9f95c-217">Specificare la sottodirectory in cui risiedono gli asset statici dell'app ( *{NOME ASSEMBLY}/dist/{PERCORSO RICHIESTO}* ).</span><span class="sxs-lookup"><span data-stu-id="9f95c-217">Serve the sub-directory where the app's static assets reside (*{ASSEMBLY NAME}/dist/{PATH REQUESTED}*).</span></span>
  * <span data-ttu-id="9f95c-218">Creare routing di fallback SPA in modo che le richieste per gli asset non file vengano reindirizzate al documento predefinito dell'app nella relativa cartella degli asset statici ( *{NOME ASSEMBLY}/dist/index.html*).</span><span class="sxs-lookup"><span data-stu-id="9f95c-218">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*{ASSEMBLY NAME}/dist/index.html*).</span></span>

#### <a name="install-the-url-rewrite-module"></a><span data-ttu-id="9f95c-219">Installare URL Rewrite Module</span><span class="sxs-lookup"><span data-stu-id="9f95c-219">Install the URL Rewrite Module</span></span>

<span data-ttu-id="9f95c-220">[URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) è necessario per riscrivere gli URL.</span><span class="sxs-lookup"><span data-stu-id="9f95c-220">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="9f95c-221">Il modulo non viene installato per impostazione predefinita e non è disponibile per l'installazione come funzionalità del servizio ruolo Server Web (IIS).</span><span class="sxs-lookup"><span data-stu-id="9f95c-221">The module isn't installed by default, and it isn't available for install as a Web Server (IIS) role service feature.</span></span> <span data-ttu-id="9f95c-222">Il modulo deve essere scaricato dal sito Web IIS.</span><span class="sxs-lookup"><span data-stu-id="9f95c-222">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="9f95c-223">Usare Installazione guidata piattaforma Web per installare il modulo:</span><span class="sxs-lookup"><span data-stu-id="9f95c-223">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="9f95c-224">In locale, passare alla [pagina di download di URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span><span class="sxs-lookup"><span data-stu-id="9f95c-224">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="9f95c-225">Per la versione in lingua inglese selezionare **WebPI** per scaricare il programma di installazione di Installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="9f95c-225">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="9f95c-226">Per altre lingue, selezionare l'architettura appropriata per il server (x86 o x64) per scaricare il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="9f95c-226">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="9f95c-227">Copiare il programma di installazione nel server.</span><span class="sxs-lookup"><span data-stu-id="9f95c-227">Copy the installer to the server.</span></span> <span data-ttu-id="9f95c-228">Eseguire il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="9f95c-228">Run the installer.</span></span> <span data-ttu-id="9f95c-229">Selezionare il pulsante **Installa** e accettare le condizioni di licenza.</span><span class="sxs-lookup"><span data-stu-id="9f95c-229">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="9f95c-230">Al termine dell'installazione non è necessario un riavvio del server.</span><span class="sxs-lookup"><span data-stu-id="9f95c-230">A server restart isn't required after the install completes.</span></span>

#### <a name="configure-the-website"></a><span data-ttu-id="9f95c-231">Configurare il sito Web</span><span class="sxs-lookup"><span data-stu-id="9f95c-231">Configure the website</span></span>

<span data-ttu-id="9f95c-232">Impostare il **percorso fisico** del sito Web sulla cartella dell'app.</span><span class="sxs-lookup"><span data-stu-id="9f95c-232">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="9f95c-233">La cartella contiene:</span><span class="sxs-lookup"><span data-stu-id="9f95c-233">The folder contains:</span></span>

* <span data-ttu-id="9f95c-234">Il file *web.config* usato da IIS per configurare il sito Web, inclusi le regole di reindirizzamento richieste e i tipi di contenuto di file.</span><span class="sxs-lookup"><span data-stu-id="9f95c-234">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="9f95c-235">Cartella degli asset statici dell'app.</span><span class="sxs-lookup"><span data-stu-id="9f95c-235">The app's static asset folder.</span></span>

#### <a name="troubleshooting"></a><span data-ttu-id="9f95c-236">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="9f95c-236">Troubleshooting</span></span>

<span data-ttu-id="9f95c-237">Se si riceve un *errore interno del server 500* e Gestione IIS genera errori durante il tentativo di accedere alla configurazione del sito Web, verificare che sia installato URL Rewrite Module.</span><span class="sxs-lookup"><span data-stu-id="9f95c-237">If a *500 - Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="9f95c-238">Quando il modulo non è installato, il file *web.config* non può essere analizzato da IIS.</span><span class="sxs-lookup"><span data-stu-id="9f95c-238">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="9f95c-239">Ciò impedisce a Gestione IIS di caricare la configurazione del sito Web e al sito Web di fornire i file statici di Blazor.</span><span class="sxs-lookup"><span data-stu-id="9f95c-239">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="9f95c-240">Per altre informazioni sulla risoluzione dei problemi relativi alle distribuzioni in IIS, vedere <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="9f95c-240">For more information on troubleshooting deployments to IIS, see <xref:host-and-deploy/iis/troubleshoot>.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="9f95c-241">Archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="9f95c-241">Azure Storage</span></span>

<span data-ttu-id="9f95c-242">L'hosting di file statici di Archiviazione di Azure consente l'hosting di app Blazor serverless.</span><span class="sxs-lookup"><span data-stu-id="9f95c-242">Azure Storage static file hosting allows serverless Blazor app hosting.</span></span> <span data-ttu-id="9f95c-243">Sono supportati nomi di dominio personalizzati, la rete per la distribuzione di contenuti (rete CDN) di Azure e HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9f95c-243">Custom domain names, the Azure Content Delivery Network (CDN), and HTTPS are supported.</span></span>

<span data-ttu-id="9f95c-244">Quando il servizio BLOB è abilitato per l'hosting di siti Web statici in un account di archiviazione:</span><span class="sxs-lookup"><span data-stu-id="9f95c-244">When the blob service is enabled for static website hosting on a storage account:</span></span>

* <span data-ttu-id="9f95c-245">Impostare **Nome del documento di indice** su `index.html`.</span><span class="sxs-lookup"><span data-stu-id="9f95c-245">Set the **Index document name** to `index.html`.</span></span>
* <span data-ttu-id="9f95c-246">Impostare **Percorso del documento di errore** su `index.html`.</span><span class="sxs-lookup"><span data-stu-id="9f95c-246">Set the **Error document path** to `index.html`.</span></span> <span data-ttu-id="9f95c-247">I componenti di Razor e altri endpoint non basati su file non risiedono in percorsi fisici nel contenuto statico archiviato dal servizio BLOB.</span><span class="sxs-lookup"><span data-stu-id="9f95c-247">Razor Components and other non-file endpoints don't reside at physical paths in the static content stored by the blob service.</span></span> <span data-ttu-id="9f95c-248">Quando viene ricevuta una richiesta per una di queste risorse che deve essere gestita dal router Blazor, l'errore *404 - Non trovato* generato dal servizio BLOB instrada la richiesta al percorso indicato in **Percorso del documento di errore**.</span><span class="sxs-lookup"><span data-stu-id="9f95c-248">When a request for one of these resources is received that the Blazor router should handle, the *404 - Not Found* error generated by the blob service routes the request to the **Error document path**.</span></span> <span data-ttu-id="9f95c-249">Viene restituito il BLOB *index.html* e il router Blazor carica ed elabora il percorso.</span><span class="sxs-lookup"><span data-stu-id="9f95c-249">The *index.html* blob is returned, and the Blazor router loads and processes the path.</span></span>

<span data-ttu-id="9f95c-250">Per altre informazioni, vedere [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website) (Hosting di siti Web statici in Archiviazione di Azure).</span><span class="sxs-lookup"><span data-stu-id="9f95c-250">For more information, see [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span></span>

### <a name="nginx"></a><span data-ttu-id="9f95c-251">Nginx</span><span class="sxs-lookup"><span data-stu-id="9f95c-251">Nginx</span></span>

<span data-ttu-id="9f95c-252">Il file *nginx.conf* seguente è semplificato per mostrare come configurare Nginx per l'invio del file *index.html* ogni volta che non riesce a trovare un file su disco corrispondente.</span><span class="sxs-lookup"><span data-stu-id="9f95c-252">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *index.html* file whenever it can't find a corresponding file on disk.</span></span>

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

<span data-ttu-id="9f95c-253">Per altre informazioni sulla configurazione del server Web Nginx in ambiente di produzione, vedere [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/) (Creazione di file di configurazione NGINX Plus e NGINX).</span><span class="sxs-lookup"><span data-stu-id="9f95c-253">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

### <a name="nginx-in-docker"></a><span data-ttu-id="9f95c-254">Nginx in Docker</span><span class="sxs-lookup"><span data-stu-id="9f95c-254">Nginx in Docker</span></span>

<span data-ttu-id="9f95c-255">Per ospitare Blazor in Docker usando Nginx, configurare il Dockerfile per l'uso dell'immagine di Nginx basata su Alpine.</span><span class="sxs-lookup"><span data-stu-id="9f95c-255">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="9f95c-256">Aggiornare il Dockerfile per copiare il file *nginx.config* nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="9f95c-256">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="9f95c-257">Aggiungere una riga al Dockerfile, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="9f95c-257">Add one line to the Dockerfile, as shown in the following example:</span></span>

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="github-pages"></a><span data-ttu-id="9f95c-258">Pagine di GitHub</span><span class="sxs-lookup"><span data-stu-id="9f95c-258">GitHub Pages</span></span>

<span data-ttu-id="9f95c-259">Per gestire le riscritture degli URL, aggiungere un file *404.html* con uno script che gestisce il reindirizzamento della richiesta alla pagina *index.html*.</span><span class="sxs-lookup"><span data-stu-id="9f95c-259">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="9f95c-260">Per un esempio di implementazione fornito dalla community, vedere [Single Page Apps for GitHub Pages](http://spa-github-pages.rafrex.com/) (App a pagina singola per pagine GitHub) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span><span class="sxs-lookup"><span data-stu-id="9f95c-260">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="9f95c-261">È possibile visualizzare un esempio d'uso dell'approccio della community in [blazor-demo/blazor-demo.github.io su GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([sito live](https://blazor-demo.github.io/)).</span><span class="sxs-lookup"><span data-stu-id="9f95c-261">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="9f95c-262">Quando si usa un sito di progetto anziché un sito dell'organizzazione, aggiungere o aggiornare il tag `<base>` in *index.html*.</span><span class="sxs-lookup"><span data-stu-id="9f95c-262">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="9f95c-263">Impostare il valore dell'attributo `href` sul nome del repository GitHub con una barra finale (ad esempio, `my-repository/`.</span><span class="sxs-lookup"><span data-stu-id="9f95c-263">Set the `href` attribute value to the GitHub repository name with a trailing slash (for example, `my-repository/`.</span></span>
