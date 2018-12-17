---
title: Usare ASP.NET Core SignalR con TypeScript e Webpack
author: ssougnez
description: In questa esercitazione verrà configurato Webpack per creare un bundle e compilare un'app Web ASP.NET Core SignalR il cui client è scritto in TypeScript.
monikerRange: '>= aspnetcore-2.2'
ms.author: scaddie
ms.custom: mvc
ms.date: 11/30/2018
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: b2d59dfc449953cc2d747b507295c00ac0f652dd
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862252"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a><span data-ttu-id="e833d-103">Usare ASP.NET Core SignalR con TypeScript e Webpack</span><span class="sxs-lookup"><span data-stu-id="e833d-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="e833d-104">Di [Sébastien Sougnez](https://twitter.com/ssougnez) e [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="e833d-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="e833d-105">[Webpack](https://webpack.js.org/) consente agli sviluppatori di creare un bundle delle risorse di un'app Web sul lato client-e di compilarle.</span><span class="sxs-lookup"><span data-stu-id="e833d-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="e833d-106">Questa esercitazione illustra l'uso di Webpack in un'app Web ASP.NET Core SignalR il cui client è scritto in [TypeScript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="e833d-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="e833d-107">In questa esercitazione si imparerà a:</span><span class="sxs-lookup"><span data-stu-id="e833d-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e833d-108">Eseguire lo scaffolding di un'app ASP.NET Core SignalR di base</span><span class="sxs-lookup"><span data-stu-id="e833d-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="e833d-109">Configurare il client TypeScript di SignalR</span><span class="sxs-lookup"><span data-stu-id="e833d-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="e833d-110">Configurare una pipeline di compilazione tramite Webpack</span><span class="sxs-lookup"><span data-stu-id="e833d-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="e833d-111">Configurare il server SignalR</span><span class="sxs-lookup"><span data-stu-id="e833d-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="e833d-112">Abilitare la comunicazione tra client e server</span><span class="sxs-lookup"><span data-stu-id="e833d-112">Enable communication between client and server</span></span>

<span data-ttu-id="e833d-113">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e833d-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-vs-vsc-2.2.md)]

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="e833d-114">Creare l'app Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e833d-114">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e833d-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e833d-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e833d-116">Configurare Visual Studio in modo che cerchi npm nella variabile di ambiente *PATH*.</span><span class="sxs-lookup"><span data-stu-id="e833d-116">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="e833d-117">Per impostazione predefinita, Visual Studio usa la versione di npm trovata nella directory di installazione.</span><span class="sxs-lookup"><span data-stu-id="e833d-117">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="e833d-118">Seguire queste istruzioni in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="e833d-118">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="e833d-119">Passare a **Strumenti** > **Opzioni** > **Progetti e soluzioni** > **Gestione pacchetti Web** > **Strumenti Web esterni**.</span><span class="sxs-lookup"><span data-stu-id="e833d-119">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="e833d-120">Selezionare la voce *$(PATH)* dall'elenco.</span><span class="sxs-lookup"><span data-stu-id="e833d-120">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="e833d-121">Fare clic sulla freccia in su per spostare la voce nella seconda posizione nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="e833d-121">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Configurazione di Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="e833d-123">La configurazione di Visual Studio è completata.</span><span class="sxs-lookup"><span data-stu-id="e833d-123">Visual Studio configuration is completed.</span></span> <span data-ttu-id="e833d-124">A questo punto è possibile creare il progetto</span><span class="sxs-lookup"><span data-stu-id="e833d-124">It's time to create the project.</span></span>

1. <span data-ttu-id="e833d-125">Selezionare l'opzione di menu **File** > **Nuovo** > **Progetto** e scegliere il modello **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="e833d-125">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="e833d-126">Assegnare al progetto il nome *SignalRWebPack* e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="e833d-126">Name the project *SignalRWebPack*, and select **OK**.</span></span>
1. <span data-ttu-id="e833d-127">Selezionare *.NET Core* nell'elenco a discesa del framework di destinazione e quindi selezionare *ASP.NET Core 2.2* nell'elenco a discesa del selettore del framework.</span><span class="sxs-lookup"><span data-stu-id="e833d-127">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.2* from the framework selector drop-down.</span></span> <span data-ttu-id="e833d-128">Selezionare il modello **Vuoto** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="e833d-128">Select the **Empty** template, and select **OK**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e833d-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e833d-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e833d-130">Eseguire il comando seguente nel **Terminale integrato**:</span><span class="sxs-lookup"><span data-stu-id="e833d-130">Run the following command in the **Integrated Terminal**:</span></span>

```console
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="e833d-131">Nella directory *SignalRWebPack* viene creata un'app Web ASP.NET Core vuota destinata a .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e833d-131">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="e833d-132">Configurare Webpack e TypeScript</span><span class="sxs-lookup"><span data-stu-id="e833d-132">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="e833d-133">I passaggi seguenti consentono di configurare la conversione da TypeScript a JavaScript e di creare un bundle delle risorse sul lato client.</span><span class="sxs-lookup"><span data-stu-id="e833d-133">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="e833d-134">Eseguire il comando seguente nella radice del progetto per creare un file *package.json*:</span><span class="sxs-lookup"><span data-stu-id="e833d-134">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="e833d-135">Aggiungere la proprietà evidenziata al file *package.json*:</span><span class="sxs-lookup"><span data-stu-id="e833d-135">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    <span data-ttu-id="e833d-136">Impostando la proprietà `private` su `true` si evita la visualizzazione di avvisi di installazione del pacchetto nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="e833d-136">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="e833d-137">Installare i pacchetti npm necessari.</span><span class="sxs-lookup"><span data-stu-id="e833d-137">Install the required npm packages.</span></span> <span data-ttu-id="e833d-138">Eseguire il comando seguente dalla radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="e833d-138">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@0.1.19 css-loader@0.28.11 html-webpack-plugin@3.2.0 mini-css-extract-plugin@0.4.0 ts-loader@4.4.1 typescript@2.9.2 webpack@4.12.0 webpack-cli@3.0.6
    ```

    <span data-ttu-id="e833d-139">Ecco alcuni dettagli del comando da tenere in considerazione:</span><span class="sxs-lookup"><span data-stu-id="e833d-139">Some command details to note:</span></span>

    * <span data-ttu-id="e833d-140">Un numero di versione segue il segno `@` per ogni nome di pacchetto.</span><span class="sxs-lookup"><span data-stu-id="e833d-140">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="e833d-141">npm installa queste versioni del pacchetto specifiche.</span><span class="sxs-lookup"><span data-stu-id="e833d-141">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="e833d-142">L'opzione `-E` disabilita il comportamento predefinito di npm, in base al quale gli operatori intervallo di [Versionamento Semantico](https://semver.org/) vengono scritti in *package.json*.</span><span class="sxs-lookup"><span data-stu-id="e833d-142">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="e833d-143">Ad esempio, viene usato `"webpack": "4.12.0"` invece di `"webpack": "^4.12.0"`.</span><span class="sxs-lookup"><span data-stu-id="e833d-143">For example, `"webpack": "4.12.0"` is used instead of `"webpack": "^4.12.0"`.</span></span> <span data-ttu-id="e833d-144">Questa opzione impedisce che vengano eseguiti aggiornamenti non desiderati a versioni più recenti del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="e833d-144">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="e833d-145">Per informazioni dettagliate, vedere la documentazione ufficiale di [npm-install](https://docs.npmjs.com/cli/install).</span><span class="sxs-lookup"><span data-stu-id="e833d-145">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="e833d-146">Sostituire la proprietà `scripts` del file *package.json* con il frammento di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="e833d-146">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="e833d-147">Ecco una spiegazione degli script:</span><span class="sxs-lookup"><span data-stu-id="e833d-147">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="e833d-148">`build`: crea un bundle delle risorse sul lato client in modalità di sviluppo e verifica la presenza di modifiche al file.</span><span class="sxs-lookup"><span data-stu-id="e833d-148">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="e833d-149">Questo controllo fa sì che il bundle venga rigenerato a ogni modifica del file di progetto.</span><span class="sxs-lookup"><span data-stu-id="e833d-149">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="e833d-150">L'opzione `mode` disabilita le ottimizzazioni di produzione, come l'eliminazione del codice non utilizzato e la minimizzazione.</span><span class="sxs-lookup"><span data-stu-id="e833d-150">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="e833d-151">Usare `build` solo in modalità di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="e833d-151">Only use `build` in development.</span></span>
    * <span data-ttu-id="e833d-152">`release`: crea un bundle delle risorse sul lato client in modalità di produzione.</span><span class="sxs-lookup"><span data-stu-id="e833d-152">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="e833d-153">`publish`: esegue lo script `release` per creare il bundle delle risorse sul lato client in modalità di produzione.</span><span class="sxs-lookup"><span data-stu-id="e833d-153">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="e833d-154">Chiama il comando [publish](/dotnet/core/tools/dotnet-publish) dell'interfaccia della riga di comando di .NET Core per pubblicare l'app.</span><span class="sxs-lookup"><span data-stu-id="e833d-154">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="e833d-155">Creare un file denominato *webpack.config.js* nella radice del progetto con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="e833d-155">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    <span data-ttu-id="e833d-156">Il file precedente configura la compilazione di Webpack.</span><span class="sxs-lookup"><span data-stu-id="e833d-156">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="e833d-157">Ecco alcuni dettagli di configurazione del server da tenere in considerazione:</span><span class="sxs-lookup"><span data-stu-id="e833d-157">Some configuration details to note:</span></span>

    * <span data-ttu-id="e833d-158">La proprietà `output` esegue l'override del valore predefinito di *dist*.</span><span class="sxs-lookup"><span data-stu-id="e833d-158">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="e833d-159">Il bundle viene invece creato nella directory *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="e833d-159">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="e833d-160">La matrice `resolve.extensions` include *.js* per importare il codice JavaScript del client SignalR.</span><span class="sxs-lookup"><span data-stu-id="e833d-160">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="e833d-161">Creare una nuova directory *src* nella radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="e833d-161">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="e833d-162">La sua funzione è quella di archiviare le risorse lato client del progetto.</span><span class="sxs-lookup"><span data-stu-id="e833d-162">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="e833d-163">Creare *src/index.html* con il contenuto seguente.</span><span class="sxs-lookup"><span data-stu-id="e833d-163">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    <span data-ttu-id="e833d-164">Il codice HTML precedente definisce il markup del boilerplate della home page.</span><span class="sxs-lookup"><span data-stu-id="e833d-164">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="e833d-165">Creare una nuova directory *src/css*.</span><span class="sxs-lookup"><span data-stu-id="e833d-165">Create a new *src/css* directory.</span></span> <span data-ttu-id="e833d-166">La sua funzione è quella di archiviare i file *.css* del progetto.</span><span class="sxs-lookup"><span data-stu-id="e833d-166">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="e833d-167">Creare *src/css/main.css* con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="e833d-167">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    <span data-ttu-id="e833d-168">Il file *main.css* precedente definisce lo stile dell'app.</span><span class="sxs-lookup"><span data-stu-id="e833d-168">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="e833d-169">Creare *src/tsconfig.json* con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="e833d-169">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    <span data-ttu-id="e833d-170">Il codice precedente configura il compilatore TypeScript in modo da produrre codice JavaScript compatibile con [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5.</span><span class="sxs-lookup"><span data-stu-id="e833d-170">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="e833d-171">Creare *src/index.ts* con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="e833d-171">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="e833d-172">Il compilatore TypeScript precedente recupera i riferimenti agli elementi DOM e collega due gestori eventi:</span><span class="sxs-lookup"><span data-stu-id="e833d-172">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="e833d-173">`keyup`: questo evento viene generato quando l'utente digita qualcosa nella casella di testo identificata come `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="e833d-173">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="e833d-174">La funzione `send` viene chiamata quando l'utente preme **INVIO**.</span><span class="sxs-lookup"><span data-stu-id="e833d-174">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="e833d-175">`click`: questo evento viene generato quando l'utente fa clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="e833d-175">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="e833d-176">Viene chiamata la funzione `send`.</span><span class="sxs-lookup"><span data-stu-id="e833d-176">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="e833d-177">Configurare l'app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e833d-177">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="e833d-178">Il codice fornito nel metodo `Startup.Configure` visualizza *Hello World!*.</span><span class="sxs-lookup"><span data-stu-id="e833d-178">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="e833d-179">Sostituire la chiamata del metodo `app.Run` con chiamate a [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="e833d-179">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="e833d-180">Il codice precedente consente al server di individuare e servire il file *index.html*, sia che l'utente immetta l'URL completo o l'URL radice dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="e833d-180">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="e833d-181">Chiamare [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) nel metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="e833d-181">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="e833d-182">Aggiunge i servizi SignalR al progetto.</span><span class="sxs-lookup"><span data-stu-id="e833d-182">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="e833d-183">Eseguire il mapping di una route */hub* all'hub `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="e833d-183">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="e833d-184">Aggiungere le righe seguenti alla fine del metodo `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="e833d-184">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="e833d-185">Creare una nuova directory denominata *Hubs* nella radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="e833d-185">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="e833d-186">La sua funzione è quella di archiviare l'hub SignalR, che verrà creato nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="e833d-186">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="e833d-187">Creare l'hub *Hubs/ChatHub.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="e833d-187">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="e833d-188">Aggiungere il codice seguente all'inizio del file *Startup.cs* per risolvere il riferimento a `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="e833d-188">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="e833d-189">Abilitare la comunicazione tra client e server</span><span class="sxs-lookup"><span data-stu-id="e833d-189">Enable client and server communication</span></span>

<span data-ttu-id="e833d-190">L'app attualmente visualizza un semplice form per l'invio di messaggi.</span><span class="sxs-lookup"><span data-stu-id="e833d-190">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="e833d-191">Quando si prova a eseguire questa operazione, non accade nulla.</span><span class="sxs-lookup"><span data-stu-id="e833d-191">Nothing happens when you try to do so.</span></span> <span data-ttu-id="e833d-192">Il server è in ascolto di una route specifica, ma non esegue alcuna operazione con i messaggi inviati.</span><span class="sxs-lookup"><span data-stu-id="e833d-192">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="e833d-193">Eseguire il comando seguente nella radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="e833d-193">Execute the following command at the project root:</span></span>

    ```console
    npm install @aspnet/signalr
    ```

    <span data-ttu-id="e833d-194">Il comando precedente installa il [client TypeScript di SignalR](https://www.npmjs.com/package/@aspnet/signalr), che consente al client di inviare messaggi al server.</span><span class="sxs-lookup"><span data-stu-id="e833d-194">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@aspnet/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="e833d-195">Aggiungere il codice evidenziato al file *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="e833d-195">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="e833d-196">Il codice precedente supporta la ricezione di messaggi dal server.</span><span class="sxs-lookup"><span data-stu-id="e833d-196">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="e833d-197">La classe `HubConnectionBuilder` crea un nuovo compilatore per la configurazione della connessione al server.</span><span class="sxs-lookup"><span data-stu-id="e833d-197">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="e833d-198">La funzione `withUrl` configura l'URL dell'hub.</span><span class="sxs-lookup"><span data-stu-id="e833d-198">The `withUrl` function configures the hub URL.</span></span>

    <span data-ttu-id="e833d-199">SignalR consente lo scambio di messaggi tra un client e un server.</span><span class="sxs-lookup"><span data-stu-id="e833d-199">SignalR enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="e833d-200">Ogni messaggio ha un nome specifico.</span><span class="sxs-lookup"><span data-stu-id="e833d-200">Each message has a specific name.</span></span> <span data-ttu-id="e833d-201">Ad esempio, i messaggi con nome `messageReceived` eseguono la logica responsabile della visualizzazione del nuovo messaggio nell'area dei messaggi.</span><span class="sxs-lookup"><span data-stu-id="e833d-201">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="e833d-202">L'ascolto di un messaggio specifico può essere eseguito tramite la funzione `on`.</span><span class="sxs-lookup"><span data-stu-id="e833d-202">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="e833d-203">È possibile restare in ascolto di un numero indefinito di nomi di messaggio.</span><span class="sxs-lookup"><span data-stu-id="e833d-203">You can listen to any number of message names.</span></span> <span data-ttu-id="e833d-204">Si può anche passare parametri al messaggio, come il nome dell'autore e il contenuto del messaggio ricevuto.</span><span class="sxs-lookup"><span data-stu-id="e833d-204">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="e833d-205">Quando il client riceve un messaggio, viene creato un nuovo elemento `div` con il nome dell'autore e il contenuto del messaggio nell'attributo `innerHTML`.</span><span class="sxs-lookup"><span data-stu-id="e833d-205">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="e833d-206">Viene aggiunto all'elemento `div` principale che visualizza i messaggi.</span><span class="sxs-lookup"><span data-stu-id="e833d-206">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="e833d-207">Ora che il client può ricevere messaggi, configurarlo anche per l'invio.</span><span class="sxs-lookup"><span data-stu-id="e833d-207">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="e833d-208">Aggiungere il codice evidenziato al file *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="e833d-208">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    <span data-ttu-id="e833d-209">Per inviare un messaggio attraverso la connessione WebSockets è necessario chiamare il metodo `send`.</span><span class="sxs-lookup"><span data-stu-id="e833d-209">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="e833d-210">Il primo parametro del metodo è il nome del messaggio.</span><span class="sxs-lookup"><span data-stu-id="e833d-210">The method's first parameter is the message name.</span></span> <span data-ttu-id="e833d-211">I dati del messaggio sono rappresentati dagli altri parametri.</span><span class="sxs-lookup"><span data-stu-id="e833d-211">The message data inhabits the other parameters.</span></span> <span data-ttu-id="e833d-212">In questo esempio un messaggio identificato come `newMessage` viene inviato al server.</span><span class="sxs-lookup"><span data-stu-id="e833d-212">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="e833d-213">Il messaggio è costituito dal nome utente e dall'input dell'utente in una casella di testo.</span><span class="sxs-lookup"><span data-stu-id="e833d-213">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="e833d-214">Se l'invio riesce, il valore della casella di testo viene cancellato.</span><span class="sxs-lookup"><span data-stu-id="e833d-214">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="e833d-215">Aggiungere il metodo evidenziato alla classe `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="e833d-215">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="e833d-216">Il codice precedente trasmette i messaggi ricevuti a tutti gli utenti connessi dopo che il server li riceve.</span><span class="sxs-lookup"><span data-stu-id="e833d-216">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="e833d-217">Non è necessario avere un metodo generico `on` per ricevere tutti i messaggi.</span><span class="sxs-lookup"><span data-stu-id="e833d-217">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="e833d-218">È sufficiente un metodo denominato dopo il nome del messaggio.</span><span class="sxs-lookup"><span data-stu-id="e833d-218">A method named after the message name suffices.</span></span>

    <span data-ttu-id="e833d-219">In questo esempio il client TypeScript invia un messaggio identificato come `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="e833d-219">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="e833d-220">Il metodo C# `NewMessage` si aspetta i dati inviati dal client.</span><span class="sxs-lookup"><span data-stu-id="e833d-220">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="e833d-221">Viene eseguita una chiamata al metodo [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) su [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="e833d-221">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="e833d-222">I messaggi ricevuti vengono inviati a tutti i client connessi all'hub.</span><span class="sxs-lookup"><span data-stu-id="e833d-222">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="e833d-223">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="e833d-223">Test the app</span></span>

<span data-ttu-id="e833d-224">Per verificare che l'app funzioni, eseguire la procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="e833d-224">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e833d-225">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e833d-225">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="e833d-226">Eseguire Webpack in modalità *versione*.</span><span class="sxs-lookup"><span data-stu-id="e833d-226">Run Webpack in *release* mode.</span></span> <span data-ttu-id="e833d-227">Nella finestra **Console di gestione pacchetti** eseguire il comando seguente nella radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="e833d-227">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="e833d-228">Se non ci si trova nella directory radice del progetto, immettere `cd SignalRWebPack` prima di immettere il comando.</span><span class="sxs-lookup"><span data-stu-id="e833d-228">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="e833d-229">Selezionare **Debug** > **Avvia senza eseguire il debug** per avviare l'app in un browser senza collegare il debugger.</span><span class="sxs-lookup"><span data-stu-id="e833d-229">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="e833d-230">Il file *wwwroot/index.html* viene servito all'indirizzo `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="e833d-230">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="e833d-231">Aprire un'altra istanza del browser (qualsiasi browser).</span><span class="sxs-lookup"><span data-stu-id="e833d-231">Open another browser instance (any browser).</span></span> <span data-ttu-id="e833d-232">Incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="e833d-232">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="e833d-233">Scegliere un browser, digitare qualcosa nella casella di testo **Messaggio** e fare clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="e833d-233">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="e833d-234">Il nome utente e il messaggio univoci vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="e833d-234">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e833d-235">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e833d-235">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="e833d-236">Eseguire Webpack in modalità *versione* eseguendo il comando seguente nella radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="e833d-236">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="e833d-237">Compilare ed eseguire l'app eseguendo il comando seguente nella radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="e833d-237">Build and run the app by executing the following command in the project root:</span></span>

    ```console
    dotnet run
    ```

    <span data-ttu-id="e833d-238">Il server Web avvia l'app e la rende disponibile su localhost.</span><span class="sxs-lookup"><span data-stu-id="e833d-238">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="e833d-239">Aprire un browser all'indirizzo `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="e833d-239">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="e833d-240">Il file *wwwroot/index.html* viene servito.</span><span class="sxs-lookup"><span data-stu-id="e833d-240">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="e833d-241">Copiare l'URL dalla barra dell'indirizzo.</span><span class="sxs-lookup"><span data-stu-id="e833d-241">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="e833d-242">Aprire un'altra istanza del browser (qualsiasi browser).</span><span class="sxs-lookup"><span data-stu-id="e833d-242">Open another browser instance (any browser).</span></span> <span data-ttu-id="e833d-243">Incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="e833d-243">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="e833d-244">Scegliere un browser, digitare qualcosa nella casella di testo **Messaggio** e fare clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="e833d-244">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="e833d-245">Il nome utente e il messaggio univoci vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="e833d-245">The unique user name and message are displayed on both pages instantly.</span></span>

---

![Messaggio visualizzato in entrambe le finestre del browser](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a><span data-ttu-id="e833d-247">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e833d-247">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
