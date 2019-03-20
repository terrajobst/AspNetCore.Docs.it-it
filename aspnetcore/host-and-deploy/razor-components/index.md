---
title: Ospitare e distribuire Razor Components
author: guardrex
description: 'Informazioni su come ospitare e distribuire Razor Components e app Blazor con ASP.NET Core, reti per la distribuzione di contenuti (CDN), file server e pagine di GitHub.'
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: host-and-deploy/razor-components/index
---
# <a name="host-and-deploy-razor-components"></a><span data-ttu-id="9b7f5-103">Ospitare e distribuire Razor Components</span><span class="sxs-lookup"><span data-stu-id="9b7f5-103">Host and deploy Razor Components</span></span>

<span data-ttu-id="9b7f5-104">Di [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="9b7f5-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

## <a name="publish-the-app"></a><span data-ttu-id="9b7f5-105">Pubblicare l'app</span><span class="sxs-lookup"><span data-stu-id="9b7f5-105">Publish the app</span></span>

<span data-ttu-id="9b7f5-106">Le app vengono pubblicate per la distribuzione nella configurazione di rilascio con il comando [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="9b7f5-106">Apps are published for deployment in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="9b7f5-107">Un IDE può gestire l'esecuzione del comando `dotnet publish` automaticamente usando le funzionalità di pubblicazione predefinite, quindi potrebbe non essere necessario eseguire manualmente il comando da un prompt dei comandi a seconda degli strumenti di sviluppo in uso.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-107">An IDE may handle executing the `dotnet publish` command automatically using its built-in publishing features, so it might not be necessary to manually execute the command from a command prompt depending on the development tools in use.</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="9b7f5-108">`dotnet publish` attiva un [ripristino](/dotnet/core/tools/dotnet-restore) delle dipendenze del progetto e [compila](/dotnet/core/tools/dotnet-build) il progetto prima di creare gli asset per la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-108">`dotnet publish` triggers a [restore](/dotnet/core/tools/dotnet-restore) of the project's dependencies and [builds](/dotnet/core/tools/dotnet-build) the project before creating the assets for deployment.</span></span> <span data-ttu-id="9b7f5-109">Come parte del processo di compilazione, vengono rimossi gli assembly e i metodi non usati per ridurre le dimensioni del download e i tempi di caricamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-109">As part of the build process, unused methods and assemblies are removed to reduce app download size and load times.</span></span> <span data-ttu-id="9b7f5-110">La distribuzione viene creata nella cartella */bin/Release/&lt;target-framework&gt;/publish*.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-110">The deployment is created in the */bin/Release/&lt;target-framework&gt;/publish* folder.</span></span>

<span data-ttu-id="9b7f5-111">Gli asset nella cartella *publish* vengono distribuiti nel server Web.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-111">The assets in the *publish* folder are deployed to the web server.</span></span> <span data-ttu-id="9b7f5-112">La distribuzione potrebbe essere un processo manuale o automatizzato a seconda degli strumenti di sviluppo in uso.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-112">Deployment might be a manual or automated process depending on the development tools in use.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="9b7f5-113">Valori di configurazione dell'host</span><span class="sxs-lookup"><span data-stu-id="9b7f5-113">Host configuration values</span></span>

<span data-ttu-id="9b7f5-114">Le app di Razor Components che usano il [modello di hosting sul lato server](xref:razor-components/hosting-models#server-side-hosting-model) possono accettare [valori di configurazione dell'host Web](xref:fundamentals/host/web-host#host-configuration-values).</span><span class="sxs-lookup"><span data-stu-id="9b7f5-114">Razor Components apps that use the [server-side hosting model](xref:razor-components/hosting-models#server-side-hosting-model) can accept [Web Host configuration values](xref:fundamentals/host/web-host#host-configuration-values).</span></span>

<span data-ttu-id="9b7f5-115">Le app Blazor che usano il [modello di hosting sul lato client](xref:razor-components/hosting-models#client-side-hosting-model) possono accettare i valori di configurazione dell'host seguenti come argomenti della riga di comando in fase di esecuzione nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-115">Blazor apps that use the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="9b7f5-116">Radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="9b7f5-116">Content Root</span></span>

<span data-ttu-id="9b7f5-117">L'argomento `--contentroot` imposta il percorso assoluto sulla directory che contiene i file di contenuto dell'app.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-117">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files.</span></span>

* <span data-ttu-id="9b7f5-118">Passare l'argomento quando si esegue localmente l'app a un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-118">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="9b7f5-119">Dalla directory dell'app, eseguire:</span><span class="sxs-lookup"><span data-stu-id="9b7f5-119">From the app's directory, execute:</span></span>

  ```console
  dotnet run --contentroot=/<content-root>
  ```

* <span data-ttu-id="9b7f5-120">Aggiungere una voce al file *launchSettings.json* dell'app nel profilo **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-120">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="9b7f5-121">Questa impostazione viene prelevata durante l'esecuzione dell'app con il debugger di Visual Studio e quando si esegue l'app da un prompt dei comandi con `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-121">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/<content-root>"
  ```

* <span data-ttu-id="9b7f5-122">In Visual Studio specificare l'argomento in **Proprietà** > **Debug** > **Argomenti applicazione**.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-122">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="9b7f5-123">Impostando l'argomento nella pagina delle proprietà di Visual Studio, si aggiunge l'argomento al file *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-123">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --contentroot=/<content-root>
  ```

### <a name="path-base"></a><span data-ttu-id="9b7f5-124">Base del percorso</span><span class="sxs-lookup"><span data-stu-id="9b7f5-124">Path base</span></span>

<span data-ttu-id="9b7f5-125">L'argomento `--pathbase` imposta il percorso di base dell'app per un'app eseguita in locale con un percorso virtuale non radice (il tag `<base>` `href` è impostato su un percorso diverso da `/` per staging e produzione).</span><span class="sxs-lookup"><span data-stu-id="9b7f5-125">The `--pathbase` argument sets the app base path for an app run locally with a non-root virtual path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="9b7f5-126">Per altre informazioni, vedere la sezione [Percorso di base dell'app](#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="9b7f5-126">For more information, see the [App base path](#app-base-path) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9b7f5-127">A differenza del percorso specificato per `href` del tag `<base>`, non includere una barra finale (`/`) quando si passa il valore dell'argomento `--pathbase`.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-127">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="9b7f5-128">Se il percorso di base dell'app non viene specificato nel tag `<base>` come `<base href="/CoolApp/" />` (include una barra finale), passare il valore dell'argomento della riga di comando come `--pathbase=/CoolApp` (senza barra finale).</span><span class="sxs-lookup"><span data-stu-id="9b7f5-128">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/" />` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="9b7f5-129">Passare l'argomento quando si esegue localmente l'app a un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-129">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="9b7f5-130">Dalla directory dell'app, eseguire:</span><span class="sxs-lookup"><span data-stu-id="9b7f5-130">From the app's directory, execute:</span></span>

  ```console
  dotnet run --pathbase=/<virtual-path>
  ```

* <span data-ttu-id="9b7f5-131">Aggiungere una voce al file *launchSettings.json* dell'app nel profilo **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-131">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="9b7f5-132">Questa impostazione viene prelevata durante l'esecuzione dell'app con il debugger di Visual Studio e quando si esegue l'app da un prompt dei comandi con `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-132">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/<virtual-path>"
  ```

* <span data-ttu-id="9b7f5-133">In Visual Studio specificare l'argomento in **Proprietà** > **Debug** > **Argomenti applicazione**.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-133">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="9b7f5-134">Impostando l'argomento nella pagina delle proprietà di Visual Studio, si aggiunge l'argomento al file *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-134">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --pathbase=/<virtual-path>
  ```

### <a name="urls"></a><span data-ttu-id="9b7f5-135">URL</span><span class="sxs-lookup"><span data-stu-id="9b7f5-135">URLs</span></span>

<span data-ttu-id="9b7f5-136">L'argomento `--urls` indica gli indirizzi IP o gli indirizzi host con le porte e i protocolli su cui eseguire l'ascolto per le richieste.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-136">The `--urls` argument indicates the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="9b7f5-137">Passare l'argomento quando si esegue localmente l'app a un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-137">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="9b7f5-138">Dalla directory dell'app, eseguire:</span><span class="sxs-lookup"><span data-stu-id="9b7f5-138">From the app's directory, execute:</span></span>

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="9b7f5-139">Aggiungere una voce al file *launchSettings.json* dell'app nel profilo **IIS Express**.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-139">Add an entry to the app's *launchSettings.json* file in the **IIS Express** profile.</span></span> <span data-ttu-id="9b7f5-140">Questa impostazione viene prelevata durante l'esecuzione dell'app con il debugger di Visual Studio e quando si esegue l'app da un prompt dei comandi con `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-140">This setting is picked up when running the app with the Visual Studio Debugger and when running the app from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="9b7f5-141">In Visual Studio specificare l'argomento in **Proprietà** > **Debug** > **Argomenti applicazione**.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-141">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="9b7f5-142">Impostando l'argomento nella pagina delle proprietà di Visual Studio, si aggiunge l'argomento al file *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-142">Setting the argument in the Visual Studio property page adds the argument to the *launchSettings.json* file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deploy-a-client-side-blazor-app"></a><span data-ttu-id="9b7f5-143">Distribuire un'app Blazor sul lato client</span><span class="sxs-lookup"><span data-stu-id="9b7f5-143">Deploy a client-side Blazor app</span></span>

<span data-ttu-id="9b7f5-144">Con il [modello di hosting sul lato client](xref:razor-components/hosting-models#client-side-hosting-model):</span><span class="sxs-lookup"><span data-stu-id="9b7f5-144">With the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model):</span></span>

* <span data-ttu-id="9b7f5-145">L'app Blazor, le relative dipendenze e il runtime .NET vengono scaricati nel browser.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-145">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="9b7f5-146">L'app viene eseguita direttamente nel thread dell'interfaccia utente del browser.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-146">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="9b7f5-147">È supportata una delle strategie seguenti:</span><span class="sxs-lookup"><span data-stu-id="9b7f5-147">Either of the following strategies is supported:</span></span>
  * <span data-ttu-id="9b7f5-148">L'app Blazor viene fornita da un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-148">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="9b7f5-149">Vedere la sezione [Distribuzione ospitata in Blazor sul lato client con ASP.NET Core](#client-side-blazor-hosted-deployment-with-aspnet-core).</span><span class="sxs-lookup"><span data-stu-id="9b7f5-149">Covered in the [Client-side Blazor hosted deployment with ASP.NET Core](#client-side-blazor-hosted-deployment-with-aspnet-core) section.</span></span>
  * <span data-ttu-id="9b7f5-150">L'app Blazor viene posizionata in un server o un servizio Web di hosting statico, in cui non viene usato .NET per fornire l'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-150">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="9b7f5-151">Vedere la sezione [Distribuzione autonoma di Blazor sul lato client](#client-side-blazor-standalone-deployment).</span><span class="sxs-lookup"><span data-stu-id="9b7f5-151">Covered in the [Client-side Blazor standalone deployment](#client-side-blazor-standalone-deployment) section.</span></span>
  
### <a name="configure-the-linker"></a><span data-ttu-id="9b7f5-152">Configurare il linker</span><span class="sxs-lookup"><span data-stu-id="9b7f5-152">Configure the Linker</span></span>

<span data-ttu-id="9b7f5-153">Blazor esegue il collegamento di linguaggio intermedio (IL) in ogni build per rimuovere IL non necessario dagli assembly di output.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-153">Blazor performs Intermediate Language (IL) linking on each build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="9b7f5-154">È possibile controllare il collegamento degli assembly in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-154">You can control assembly linking on build.</span></span> <span data-ttu-id="9b7f5-155">Per ulteriori informazioni, vedere <xref:host-and-deploy/razor-components/configure-linker>.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-155">For more information, see <xref:host-and-deploy/razor-components/configure-linker>.</span></span>

### <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="9b7f5-156">Riscrivere gli URL per il routing corretto</span><span class="sxs-lookup"><span data-stu-id="9b7f5-156">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="9b7f5-157">Il routing delle richieste per i componenti di pagina in un'app sul lato client non è semplice come il routing delle richieste a un'app ospitata sul lato server.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-157">Routing requests for page components in a client-side app isn't as simple as routing requests to a server-side, hosted app.</span></span> <span data-ttu-id="9b7f5-158">Si consideri un'app sul lato client con due pagine:</span><span class="sxs-lookup"><span data-stu-id="9b7f5-158">Consider a client-side app with two pages:</span></span>

* <span data-ttu-id="9b7f5-159">**_Main.cshtml_** &ndash; Viene caricata nella radice dell'app e contiene un collegamento alla pagina About (`href="About"`).</span><span class="sxs-lookup"><span data-stu-id="9b7f5-159">**_Main.cshtml_** &ndash; Loads at the root of the app and contains a link to the About page (`href="About"`).</span></span>
* <span data-ttu-id="9b7f5-160">**_About.cshtml_** &ndash; Pagina About (Informazioni su).</span><span class="sxs-lookup"><span data-stu-id="9b7f5-160">**_About.cshtml_** &ndash; About page.</span></span>

<span data-ttu-id="9b7f5-161">Quando viene richiesto il documento predefinito dell'app usando la barra degli indirizzi del browser (ad esempio, `https://www.contoso.com/`):</span><span class="sxs-lookup"><span data-stu-id="9b7f5-161">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="9b7f5-162">Il browser invia una richiesta.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-162">The browser makes a request.</span></span>
1. <span data-ttu-id="9b7f5-163">Viene restituita la pagina predefinita, di solito *index.html*.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-163">The default page is returned, which is usually *index.html*.</span></span>
1. <span data-ttu-id="9b7f5-164">*index.html* esegue il bootstrap dell'app.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-164">*index.html* bootstraps the app.</span></span>
1. <span data-ttu-id="9b7f5-165">Il router di Blazor viene caricato e viene visualizzata la pagina principale di Razor (*Main.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="9b7f5-165">Blazor's router loads and the Razor Main page (*Main.cshtml*) is displayed.</span></span>

<span data-ttu-id="9b7f5-166">Nella pagina principale, se si seleziona il collegamento alla pagina About viene caricata tale pagina.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-166">On the Main page, selecting the link to the About page loads the About page.</span></span> <span data-ttu-id="9b7f5-167">La selezione del collegamento alla pagina About funziona nel client perché il router Blazor impedisce al browser di inviare una richiesta su Internet a `www.contoso.com` per `About` e fornisce la pagina About autonomamente.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-167">Selecting the link to the About page works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the About page itself.</span></span> <span data-ttu-id="9b7f5-168">Tutte le richieste di pagine interne *all'interno dell'app sul lato client* funzionano allo stesso modo: Le richieste non attivano richieste basate su browser a risorse ospitate nel server su Internet.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-168">All of the requests for internal pages *within the client-side app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="9b7f5-169">Le richieste vengono gestite internamente dal router.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-169">The router handles the requests internally.</span></span>

<span data-ttu-id="9b7f5-170">Se una richiesta viene effettuata usando la barra degli indirizzi del browser per `www.contoso.com/About`, la richiesta ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-170">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="9b7f5-171">La risorsa non esiste nell'host Internet dell'app, quindi viene restituita la risposta *404 non trovato*.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-171">No such resource exists on the app's Internet host, so a *404 Not found* response is returned.</span></span>

<span data-ttu-id="9b7f5-172">Dato che i browser inviano le richieste agli host basati su Internet per le pagine sul lato client, i server Web e i servizi di hosting devono riscrivere tutte le richieste per le risorse che non si trovano fisicamente nel server per la pagina *index.html*.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-172">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the *index.html* page.</span></span> <span data-ttu-id="9b7f5-173">Quando viene restituita la pagina *index.html*, il router sul lato client dell'app subentra e risponde con la risorsa corretta.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-173">When *index.html* is returned, the app's client-side router takes over and responds with the correct resource.</span></span>

### <a name="app-base-path"></a><span data-ttu-id="9b7f5-174">Percorso di base dell'app</span><span class="sxs-lookup"><span data-stu-id="9b7f5-174">App base path</span></span>

<span data-ttu-id="9b7f5-175">Il percorso base dell'app è il percorso radice dell'app virtuale nel server.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-175">The app base path is the virtual app root path on the server.</span></span> <span data-ttu-id="9b7f5-176">Ad esempio, un'app che si trova nel server di Contoso in una cartella virtuale in `/CoolApp/` è raggiungibile all'indirizzo `https://www.contoso.com/CoolApp` e ha un percorso virtuale di base `/CoolApp/`.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-176">For example, an app that resides on the Contoso server in a virtual folder at `/CoolApp/` is reached at `https://www.contoso.com/CoolApp` and has a virtual base path of `/CoolApp/`.</span></span> <span data-ttu-id="9b7f5-177">Impostando il percorso di base dell'app su `CoolApp/`, l'app è a conoscenza della posizione in cui risiede virtualmente nel server.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-177">By setting the app base path to `CoolApp/`, the app is made aware of where it virtually resides on the server.</span></span> <span data-ttu-id="9b7f5-178">L'app può usare il percorso di base dell'app per costruire URL relativi rispetto alla radice dell'app da un componente che non si trova nella directory radice.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-178">The app can use the app base path to construct URLs relative to the app root from a component that isn't in the root directory.</span></span> <span data-ttu-id="9b7f5-179">In questo modo i componenti presenti a diversi livelli della struttura di directory possono creare collegamenti ad altre risorse in varie posizioni in tutta l'app.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-179">This allows components that exist at different levels of the directory structure to build links to other resources at locations throughout the app.</span></span> <span data-ttu-id="9b7f5-180">Il percorso di base dell'app viene anche usato per intercettare i clic su collegamenti ipertestuali in cui la destinazione del collegamento `href` si trova all'interno dello spazio URI del percorso di base dell'app. Il router Blazor gestisce la navigazione interna.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-180">The app base path is also used to intercept hyperlink clicks where the `href` target of the link is within the app base path URI space&mdash;the Blazor router handles the internal navigation.</span></span>

<span data-ttu-id="9b7f5-181">In molti scenari di hosting, il percorso virtuale del server per l'app è la radice dell'app.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-181">In many hosting scenarios, the server's virtual path to the app is the root of the app.</span></span> <span data-ttu-id="9b7f5-182">In questi casi, il percorso di base dell'app è una barra (`<base href="/" />`), ovvero la configurazione predefinita per un'app.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-182">In these cases, the app base path is a forward slash (`<base href="/" />`), which is the default configuration for an app.</span></span> <span data-ttu-id="9b7f5-183">In altri scenari di hosting, ad esempio le pagine di GitHub, le directory virtuali di IIS e o le sottoapplicazioni, è necessario impostare il percorso di base dell'app sul percorso virtuale del server per l'app.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-183">In other hosting scenarios, such as GitHub Pages and IIS virtual directories or sub-applications, the app base path must be set to the server's virtual path to the app.</span></span> <span data-ttu-id="9b7f5-184">Per impostare il percorso di base dell'app, aggiungere o aggiornare il tag `<base>` nel file *index.html* all'interno degli elementi del tag `<head>`.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-184">To set the app's base path, add or update the `<base>` tag in *index.html* found within the `<head>` tag elements.</span></span> <span data-ttu-id="9b7f5-185">Impostare il valore dell'attributo `href` su `<virtual-path>/` (la barra finale è obbligatoria), dove `<virtual-path>/` è il percorso radice dell'app virtuale completo nel server per l'app.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-185">Set the `href` attribute value to `<virtual-path>/` (the trailing slash is required), where `<virtual-path>/` is the full virtual app root path on the server for the app.</span></span> <span data-ttu-id="9b7f5-186">Nell'esempio precedente, il percorso virtuale è impostato su `CoolApp/`: `<base href="CoolApp/" />`.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-186">In the preceding example, the virtual path is set to `CoolApp/`: `<base href="CoolApp/" />`.</span></span>

<span data-ttu-id="9b7f5-187">Per un'app con un percorso virtuale non radice configurato (ad esempio, `<base href="CoolApp/" />`), l'app non riesce a trovare le relative risorse *quando viene eseguita in locale*.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-187">For an app with a non-root virtual path configured (for example, `<base href="CoolApp/" />`), the app fails to find its resources *when run locally*.</span></span> <span data-ttu-id="9b7f5-188">Per risolvere questo problema durante sviluppo e test locali, è possibile specificare un argomento *base del percorso* corrispondente al valore `href` del tag `<base>` in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-188">To overcome this problem during local development and testing, you can supply a *path base* argument that matches the `href` value of the `<base>` tag at runtime.</span></span>

<span data-ttu-id="9b7f5-189">Per passare l'argomento di base del percorso con il percorso radice (`/`) quando si esegue l'app in locale, eseguire il comando seguente dalla directory dell'app:</span><span class="sxs-lookup"><span data-stu-id="9b7f5-189">To pass the path base argument with the root path (`/`) when running the app locally, execute the following command from the app's directory:</span></span>

```console
dotnet run --pathbase=/CoolApp
```

<span data-ttu-id="9b7f5-190">L'app risponde in locale all'indirizzo `http://localhost:port/CoolApp`.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-190">The app responds locally at `http://localhost:port/CoolApp`.</span></span>

<span data-ttu-id="9b7f5-191">Per altre informazioni, vedere la sezione [Valore di configurazione dell'host della base del percorso](#path-base).</span><span class="sxs-lookup"><span data-stu-id="9b7f5-191">For more information, see the [path base host configuration value](#path-base) section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9b7f5-192">Se un'app usa il [modello di hosting sul lato client](xref:razor-components/hosting-models#client-side-hosting-model) (basato sul modello di progetto **Blazor**) ed è ospitata come sottoapplicazione IIS in un'app ASP.NET Core, è importante disabilitare il gestore del modulo ASP.NET Core ereditato o assicurarsi che la sezione `<handlers>` dell'app radice (padre) nel file *web.config* non venga ereditata dalla sottoapplicazione.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-192">If an app uses the [client-side hosting model](xref:razor-components/hosting-models#client-side-hosting-model) (based on the **Blazor** project template) and is hosted as an IIS sub-application in an ASP.NET Core app, it's important to disable the inherited ASP.NET Core Module handler or make sure the root (parent) app's `<handlers>` section in the *web.config* file isn't inherited by the sub-app.</span></span>
>
> <span data-ttu-id="9b7f5-193">Rimuovere il gestore nel file *web.config* pubblicato dell'app aggiungendo una sezione `<handlers>` al file:</span><span class="sxs-lookup"><span data-stu-id="9b7f5-193">Remove the handler in the app's published *web.config* file by adding a `<handlers>` section to the file:</span></span>
>
> ```xml
> <handlers>
>   <remove name="aspNetCore" />
> </handlers>
> ```
>
> <span data-ttu-id="9b7f5-194">In alternativa, disabilitare l'ereditarietà della sezione `<system.webServer>` dell'app radice (padre) usando un elemento `<location>` con `inheritInChildApplications` impostato su `false`:</span><span class="sxs-lookup"><span data-stu-id="9b7f5-194">Alternatively, disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>
>
> ```xml
> <?xml version="1.0" encoding="utf-8"?>
> <configuration>
>  <location path="." inheritInChildApplications="false">
>    <system.webServer>
>      <handlers>
>        <add name="aspNetCore" ... />
>      </handlers>
>      <aspNetCore ... />
>    </system.webServer>
>  </location>
> </configuration>
> ```
>
> <span data-ttu-id="9b7f5-195">La rimozione del gestore o la disabilitazione dell'ereditarietà viene eseguita in aggiunta alla configurazione del percorso di base dell'app come descritto in questa sezione.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-195">Removing the handler or disabling inheritance is performed in addition to configuring the app's base path as described in this section.</span></span> <span data-ttu-id="9b7f5-196">Impostare il percorso di base dell'app nel file *index.html* dell'app sull'alias IIS usato durante la configurazione della sottoapp in IIS.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-196">Set the app base path in the app's *index.html* file to the IIS alias used when configuring the sub-app in IIS.</span></span>

### <a name="client-side-blazor-hosted-deployment-with-aspnet-core"></a><span data-ttu-id="9b7f5-197">Distribuzione ospitata in Blazor sul lato client con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9b7f5-197">Client-side Blazor hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="9b7f5-198">Una *distribuzione ospitata* fornisce l'app Blazor sul lato client ai browser da un'[app ASP.NET Core](xref:index) eseguita in un server.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-198">A *hosted deployment* serves the client-side Blazor app to browsers from an [ASP.NET Core app](xref:index) that runs on a server.</span></span>

<span data-ttu-id="9b7f5-199">L'app Blazor è inclusa con l'app ASP.NET Core nell'output pubblicato in modo che le due app vengano distribuite insieme.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-199">The Blazor app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="9b7f5-200">È necessario un server Web in grado di ospitare un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-200">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="9b7f5-201">Per una distribuzione ospitata, Visual Studio include il modello di progetto **Blazor (ospitato in ASP.NET Core)** (modello `blazorhosted` quando si usa il comando [dotnet new](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="9b7f5-201">For a hosted deployment, Visual Studio includes the **Blazor (ASP.NET Core hosted)** project template (`blazorhosted` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<span data-ttu-id="9b7f5-202">Per altre informazioni su hosting e distribuzione di app ASP.NET Core, vedere <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-202">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="9b7f5-203">Per informazioni sulla distribuzione in Servizio app di Azure, vedere gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="9b7f5-203">For information on deploying to Azure App Service, see the following topics:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>  
<span data-ttu-id="9b7f5-204">Informazioni su come pubblicare un'app ASP.NET Core in Servizio app di Azure con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-204">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

### <a name="client-side-blazor-standalone-deployment"></a><span data-ttu-id="9b7f5-205">Distribuzione autonoma di Blazor sul lato client</span><span class="sxs-lookup"><span data-stu-id="9b7f5-205">Client-side Blazor standalone deployment</span></span>

<span data-ttu-id="9b7f5-206">Una *distribuzione autonoma* serve l'app Blazor sul lato client come un set di file statici richiesti direttamente dai client.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-206">A *standalone deployment* serves the client-side Blazor app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="9b7f5-207">Non viene usato un server Web per fornire l'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-207">A web server isn't used to serve the Blazor app.</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-iis"></a><span data-ttu-id="9b7f5-208">Hosting autonomo di Blazor sul lato client con IIS</span><span class="sxs-lookup"><span data-stu-id="9b7f5-208">Client-side Blazor standalone hosting with IIS</span></span>

<span data-ttu-id="9b7f5-209">IIS è un file server statico per app Blazor.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-209">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="9b7f5-210">Per configurare IIS per ospitare Blazor, vedere [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis) (Creare un sito Web statico in IIS).</span><span class="sxs-lookup"><span data-stu-id="9b7f5-210">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="9b7f5-211">Gli asset pubblicati vengono creati nella cartella *\\bin\\Release\\&lt;target-framework&gt;\\publish*.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-211">Published assets are created in the *\\bin\\Release\\&lt;target-framework&gt;\\publish* folder.</span></span> <span data-ttu-id="9b7f5-212">Ospitare il contenuto della cartella *publish* nel server Web o nel servizio di hosting.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-212">Host the contents of the *publish* folder on the web server or hosting service.</span></span>

<span data-ttu-id="9b7f5-213">**web.config**</span><span class="sxs-lookup"><span data-stu-id="9b7f5-213">**web.config**</span></span>

<span data-ttu-id="9b7f5-214">Quando viene pubblicato un progetto Blazor, viene creato un file *web.config* con la configurazione di IIS seguente:</span><span class="sxs-lookup"><span data-stu-id="9b7f5-214">When a Blazor project is published, a *web.config* file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="9b7f5-215">Vengono impostati tipi MIME per le estensioni di file seguenti:</span><span class="sxs-lookup"><span data-stu-id="9b7f5-215">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="9b7f5-216">*\*.dll*: `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="9b7f5-216">*\*.dll*: `application/octet-stream`</span></span>
  * <span data-ttu-id="9b7f5-217">*\*.json*: `application/json`</span><span class="sxs-lookup"><span data-stu-id="9b7f5-217">*\*.json*: `application/json`</span></span>
  * <span data-ttu-id="9b7f5-218">*\*.wasm*: `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="9b7f5-218">*\*.wasm*: `application/wasm`</span></span>
  * <span data-ttu-id="9b7f5-219">*\*.woff*: `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="9b7f5-219">*\*.woff*: `application/font-woff`</span></span>
  * <span data-ttu-id="9b7f5-220">*\*.woff2*: `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="9b7f5-220">*\*.woff2*: `application/font-woff`</span></span>
* <span data-ttu-id="9b7f5-221">Viene abilitata la compressione HTTP per i tipi MIME seguenti:</span><span class="sxs-lookup"><span data-stu-id="9b7f5-221">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="9b7f5-222">Vengono stabilite le regole URL Rewrite Module:</span><span class="sxs-lookup"><span data-stu-id="9b7f5-222">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="9b7f5-223">Fornire la sottodirectory in cui risiedono gli asset statici dell'app (*&lt;assembly_name&gt;\\dist\\&lt;path_requested&gt;*).</span><span class="sxs-lookup"><span data-stu-id="9b7f5-223">Serve the sub-directory where the app's static assets reside (*&lt;assembly_name&gt;\\dist\\&lt;path_requested&gt;*).</span></span>
  * <span data-ttu-id="9b7f5-224">Creare routing di fallback SPA in modo che le richieste per gli asset non file vengano reindirizzate al documento predefinito dell'app nella relativa cartella degli asset statici (*&lt;assembly_name&gt;\\dist\\index.html*).</span><span class="sxs-lookup"><span data-stu-id="9b7f5-224">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (*&lt;assembly_name&gt;\\dist\\index.html*).</span></span>

<span data-ttu-id="9b7f5-225">**Installare URL Rewrite Module**</span><span class="sxs-lookup"><span data-stu-id="9b7f5-225">**Install the URL Rewrite Module**</span></span>

<span data-ttu-id="9b7f5-226">[URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) è necessario per riscrivere gli URL.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-226">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="9b7f5-227">Il modulo non è installato per impostazione predefinita e non è disponibile per l'installazione come funzionalità del servizio ruolo Server Web (IIS).</span><span class="sxs-lookup"><span data-stu-id="9b7f5-227">The module isn't installed by default, and it isn't available for install as an Web Server (IIS) role service feature.</span></span> <span data-ttu-id="9b7f5-228">Il modulo deve essere scaricato dal sito Web IIS.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-228">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="9b7f5-229">Usare Installazione guidata piattaforma Web per installare il modulo:</span><span class="sxs-lookup"><span data-stu-id="9b7f5-229">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="9b7f5-230">In locale, passare alla [pagina di download di URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span><span class="sxs-lookup"><span data-stu-id="9b7f5-230">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="9b7f5-231">Per la versione in lingua inglese selezionare **WebPI** per scaricare il programma di installazione di Installazione guidata piattaforma Web.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-231">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="9b7f5-232">Per altre lingue, selezionare l'architettura appropriata per il server (x86 o x64) per scaricare il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-232">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="9b7f5-233">Copiare il programma di installazione nel server.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-233">Copy the installer to the server.</span></span> <span data-ttu-id="9b7f5-234">Eseguire il programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-234">Run the installer.</span></span> <span data-ttu-id="9b7f5-235">Selezionare il pulsante **Installa** e accettare le condizioni di licenza.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-235">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="9b7f5-236">Al termine dell'installazione non è necessario un riavvio del server.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-236">A server restart isn't required after the install completes.</span></span>

<span data-ttu-id="9b7f5-237">**Configurare il sito Web**</span><span class="sxs-lookup"><span data-stu-id="9b7f5-237">**Configure the website**</span></span>

<span data-ttu-id="9b7f5-238">Impostare il **percorso fisico** del sito Web sulla cartella dell'app.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-238">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="9b7f5-239">La cartella contiene:</span><span class="sxs-lookup"><span data-stu-id="9b7f5-239">The folder contains:</span></span>

* <span data-ttu-id="9b7f5-240">Il file *web.config* usato da IIS per configurare il sito Web, inclusi le regole di reindirizzamento richieste e i tipi di contenuto di file.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-240">The *web.config* file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="9b7f5-241">Cartella degli asset statici dell'app.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-241">The app's static asset folder.</span></span>

<span data-ttu-id="9b7f5-242">**Risoluzione dei problemi**</span><span class="sxs-lookup"><span data-stu-id="9b7f5-242">**Troubleshooting**</span></span>

<span data-ttu-id="9b7f5-243">Se si riceve un *errore interno del server 500* e Gestione IIS genera errori durante il tentativo di accedere alla configurazione del sito Web, verificare che sia installato URL Rewrite Module.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-243">If a *500 Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="9b7f5-244">Quando il modulo non è installato, il file *web.config* non può essere analizzato da IIS.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-244">When the module isn't installed, the *web.config* file can't be parsed by IIS.</span></span> <span data-ttu-id="9b7f5-245">Ciò impedisce a Gestione IIS di caricare la configurazione del sito Web e al sito Web di fornire i file statici di Blazor.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-245">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="9b7f5-246">Per altre informazioni sulla risoluzione dei problemi relativi alle distribuzioni in IIS, vedere <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-246">For more information on troubleshooting deployments to IIS, see <xref:host-and-deploy/iis/troubleshoot>.</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-nginx"></a><span data-ttu-id="9b7f5-247">Hosting autonomo di Blazor sul lato client con Nginx</span><span class="sxs-lookup"><span data-stu-id="9b7f5-247">Client-side Blazor standalone hosting with Nginx</span></span>

<span data-ttu-id="9b7f5-248">Il file *nginx.conf* seguente è semplificato per mostrare come configurare Nginx per l'invio del file *Index.html* ogni volta che non riesce a trovare un file su disco corrispondente.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-248">The following *nginx.conf* file is simplified to show how to configure Nginx to send the *Index.html* file whenever it can't find a corresponding file on disk.</span></span>

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /Index.html =404;
        }
    }
}
```

<span data-ttu-id="9b7f5-249">Per altre informazioni sulla configurazione del server Web Nginx in ambiente di produzione, vedere [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/) (Creazione di file di configurazione NGINX Plus e NGINX).</span><span class="sxs-lookup"><span data-stu-id="9b7f5-249">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

#### <a name="client-side-blazor-standalone-hosting-with-nginx-in-docker"></a><span data-ttu-id="9b7f5-250">Hosting autonomo di Blazor sul lato client con Nginx in Docker</span><span class="sxs-lookup"><span data-stu-id="9b7f5-250">Client-side Blazor standalone hosting with Nginx in Docker</span></span>

<span data-ttu-id="9b7f5-251">Per ospitare Blazor in Docker usando Nginx, configurare il Dockerfile per l'uso dell'immagine di Nginx basata su Alpine.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-251">To host Blazor in Docker using Nginx, setup the Dockerfile to use the Alpine-based Nginx image.</span></span> <span data-ttu-id="9b7f5-252">Aggiornare il Dockerfile per copiare il file *nginx.config* nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-252">Update the Dockerfile to copy the *nginx.config* file into the container.</span></span>

<span data-ttu-id="9b7f5-253">Aggiungere una riga al Dockerfile, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="9b7f5-253">Add one line to the Dockerfile, as shown in the following example:</span></span>

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

#### <a name="client-side-blazor-standalone-hosting-with-github-pages"></a><span data-ttu-id="9b7f5-254">Hosting autonomo di Blazor sul lato client con pagine di GitHub</span><span class="sxs-lookup"><span data-stu-id="9b7f5-254">Client-side Blazor standalone hosting with GitHub Pages</span></span>

<span data-ttu-id="9b7f5-255">Per gestire le riscritture degli URL, aggiungere un file *404.html* con uno script che gestisce il reindirizzamento della richiesta alla pagina *index.html*.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-255">To handle URL rewrites, add a *404.html* file with a script that handles redirecting the request to the *index.html* page.</span></span> <span data-ttu-id="9b7f5-256">Per un esempio di implementazione fornito dalla community, vedere [Single Page Apps for GitHub Pages](http://spa-github-pages.rafrex.com/) (App a pagina singola per pagine GitHub) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span><span class="sxs-lookup"><span data-stu-id="9b7f5-256">For an example implementation provided by the community, see [Single Page Apps for GitHub Pages](http://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages on GitHub](https://github.com/rafrex/spa-github-pages#readme)).</span></span> <span data-ttu-id="9b7f5-257">È possibile visualizzare un esempio d'uso dell'approccio della community in [blazor-demo/blazor-demo.github.io su GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([sito live](https://blazor-demo.github.io/)).</span><span class="sxs-lookup"><span data-stu-id="9b7f5-257">An example using the community approach can be seen at [blazor-demo/blazor-demo.github.io on GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([live site](https://blazor-demo.github.io/)).</span></span>

<span data-ttu-id="9b7f5-258">Quando si usa un sito di progetto anziché un sito dell'organizzazione, aggiungere o aggiornare il tag `<base>` in *index.html*.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-258">When using a project site instead of an organization site, add or update the `<base>` tag in *index.html*.</span></span> <span data-ttu-id="9b7f5-259">Impostare il valore dell'attributo `href` su `<repository-name>/`, dove `<repository-name>/` è il nome del repository GitHub.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-259">Set the `href` attribute value to `<repository-name>/`, where `<repository-name>/` is the GitHub repository name.</span></span>

## <a name="deploy-a-server-side-razor-components-app"></a><span data-ttu-id="9b7f5-260">Distribuire un'app Razor Components sul lato server</span><span class="sxs-lookup"><span data-stu-id="9b7f5-260">Deploy a server-side Razor Components app</span></span>

<span data-ttu-id="9b7f5-261">Con il [modello di hosting sul lato server](xref:razor-components/hosting-models#server-side-hosting-model), Razor Components viene eseguito nel server dall'interno di un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-261">With the [server-side hosting model](xref:razor-components/hosting-models#server-side-hosting-model), Razor Components is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="9b7f5-262">Gli aggiornamenti dell'interfaccia utente, la gestione degli eventi e le chiamate JavaScript vengono gestite tramite una connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-262">UI updates, event handling, and JavaScript calls are handled over a SignalR connection.</span></span>

<span data-ttu-id="9b7f5-263">L'app è inclusa con l'app ASP.NET Core nell'output pubblicato in modo che le due app vengano distribuite insieme.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-263">The app is included with the ASP.NET Core app in the published output so that the two apps are deployed together.</span></span> <span data-ttu-id="9b7f5-264">È necessario un server Web in grado di ospitare un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-264">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="9b7f5-265">Per una distribuzione sul lato server, Visual Studio include il modello di progetto **Blazor (lato server in ASP.NET Core)** (modello `blazorserver` quando si usa il comando [dotnet new](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="9b7f5-265">For a server-side deployment, Visual Studio includes the **Blazor (Server-side in ASP.NET Core)** project template (`blazorserver` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

<span data-ttu-id="9b7f5-266">Quando si pubblica l'app ASP.NET Core, l'app Razor Components viene inclusa nell'output pubblicato in modo che l'app ASP.NET Core e l'app Razor Components possano essere distribuite insieme.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-266">When the ASP.NET Core app is published, the Razor Components app is included in the published output so that the ASP.NET Core app and the Razor Components app can be deployed together.</span></span> <span data-ttu-id="9b7f5-267">Per altre informazioni su hosting e distribuzione di app ASP.NET Core, vedere <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-267">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="9b7f5-268">Per informazioni sulla distribuzione in Servizio app di Azure, vedere gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="9b7f5-268">For information on deploying to Azure App Service, see the following topics:</span></span>

<xref:tutorials/publish-to-azure-webapp-using-vs>  
<span data-ttu-id="9b7f5-269">Informazioni su come pubblicare un'app ASP.NET Core in Servizio app di Azure con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9b7f5-269">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>
