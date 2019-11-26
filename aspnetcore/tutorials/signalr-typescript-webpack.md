---
title: Usare ASP.NET Core SignalR con TypeScript e Webpack
author: ssougnez
description: In questa esercitazione viene configurato Webpack per aggregare e compilare un ASP.NET Core SignalR app Web il cui client è scritto in TypeScript.
ms.author: bradyg
ms.custom: mvc
ms.date: 11/21/2019
no-loc:
- SignalR
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: a7c99c9e79647995886aec5b3a91584fd2f24451
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317479"
---
# <a name="use-aspnet-core-opno-locsignalr-with-typescript-and-webpack"></a><span data-ttu-id="f35e5-103">Usare ASP.NET Core SignalR con TypeScript e Webpack</span><span class="sxs-lookup"><span data-stu-id="f35e5-103">Use ASP.NET Core SignalR with TypeScript and Webpack</span></span>

<span data-ttu-id="f35e5-104">Di [Sébastien Sougnez](https://twitter.com/ssougnez) e [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="f35e5-104">By [Sébastien Sougnez](https://twitter.com/ssougnez) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="f35e5-105">[Webpack](https://webpack.js.org/) consente agli sviluppatori di creare un bundle delle risorse di un'app Web sul lato client-e di compilarle.</span><span class="sxs-lookup"><span data-stu-id="f35e5-105">[Webpack](https://webpack.js.org/) enables developers to bundle and build the client-side resources of a web app.</span></span> <span data-ttu-id="f35e5-106">Questa esercitazione illustra l'uso di Webpack in un ASP.NET Core SignalR app Web il cui client è scritto in [typescript](https://www.typescriptlang.org/).</span><span class="sxs-lookup"><span data-stu-id="f35e5-106">This tutorial demonstrates using Webpack in an ASP.NET Core SignalR web app whose client is written in [TypeScript](https://www.typescriptlang.org/).</span></span>

<span data-ttu-id="f35e5-107">In questa esercitazione si imparerà a:</span><span class="sxs-lookup"><span data-stu-id="f35e5-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f35e5-108">Impalcatura uno starter ASP.NET Core SignalR app</span><span class="sxs-lookup"><span data-stu-id="f35e5-108">Scaffold a starter ASP.NET Core SignalR app</span></span>
> * <span data-ttu-id="f35e5-109">Configurare il client TypeScript di SignalR</span><span class="sxs-lookup"><span data-stu-id="f35e5-109">Configure the SignalR TypeScript client</span></span>
> * <span data-ttu-id="f35e5-110">Configurare una pipeline di compilazione tramite Webpack</span><span class="sxs-lookup"><span data-stu-id="f35e5-110">Configure a build pipeline using Webpack</span></span>
> * <span data-ttu-id="f35e5-111">Configurare il server di SignalR</span><span class="sxs-lookup"><span data-stu-id="f35e5-111">Configure the SignalR server</span></span>
> * <span data-ttu-id="f35e5-112">Abilitare la comunicazione tra client e server</span><span class="sxs-lookup"><span data-stu-id="f35e5-112">Enable communication between client and server</span></span>

<span data-ttu-id="f35e5-113">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f35e5-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="f35e5-114">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="f35e5-114">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f35e5-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f35e5-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f35e5-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) con il carico di lavoro **Sviluppo ASP.NET e Web**</span><span class="sxs-lookup"><span data-stu-id="f35e5-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="f35e5-117">.NET Core SDK 3.0 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="f35e5-117">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="f35e5-118">[Node.js](https://nodejs.org/) con [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="f35e5-118">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f35e5-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f35e5-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="f35e5-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f35e5-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="f35e5-121">.NET Core SDK 3.0 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="f35e5-121">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="f35e5-122">C# per Visual Studio Code 1.17.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="f35e5-122">C# for Visual Studio Code version 1.17.1 or later</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="f35e5-123">[Node.js](https://nodejs.org/) con [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="f35e5-123">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="f35e5-124">Creare l'app Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f35e5-124">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f35e5-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f35e5-125">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f35e5-126">Configurare Visual Studio in modo che cerchi npm nella variabile di ambiente *PATH*.</span><span class="sxs-lookup"><span data-stu-id="f35e5-126">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="f35e5-127">Per impostazione predefinita, Visual Studio usa la versione di npm trovata nella directory di installazione.</span><span class="sxs-lookup"><span data-stu-id="f35e5-127">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="f35e5-128">Seguire queste istruzioni in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="f35e5-128">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="f35e5-129">Passare a **strumenti** > **Opzioni** > **progetti e soluzioni** > **Gestione pacchetti Web** > **strumenti Web esterni**.</span><span class="sxs-lookup"><span data-stu-id="f35e5-129">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="f35e5-130">Selezionare la voce *$(PATH)* dall'elenco.</span><span class="sxs-lookup"><span data-stu-id="f35e5-130">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="f35e5-131">Fare clic sulla freccia in su per spostare la voce nella seconda posizione nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="f35e5-131">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Configurazione di Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="f35e5-133">La configurazione di Visual Studio è completata.</span><span class="sxs-lookup"><span data-stu-id="f35e5-133">Visual Studio configuration is completed.</span></span> <span data-ttu-id="f35e5-134">A questo punto è possibile creare il progetto</span><span class="sxs-lookup"><span data-stu-id="f35e5-134">It's time to create the project.</span></span>

1. <span data-ttu-id="f35e5-135">Utilizzare l'opzione di menu **File** > **nuovo** > **progetto** e scegliere il modello **applicazione Web ASP.NET Core** .</span><span class="sxs-lookup"><span data-stu-id="f35e5-135">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="f35e5-136">Assegnare al progetto il nome *SignalRWebPack*e selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="f35e5-136">Name the project *SignalRWebPack*, and select **Create**.</span></span>
1. <span data-ttu-id="f35e5-137">Selezionare *.NET Core* dall'elenco a discesa Framework di destinazione e selezionare *ASP.NET Core 3,0* dall'elenco a discesa del selettore del Framework.</span><span class="sxs-lookup"><span data-stu-id="f35e5-137">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 3.0* from the framework selector drop-down.</span></span> <span data-ttu-id="f35e5-138">Selezionare il modello **vuoto** e selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="f35e5-138">Select the **Empty** template, and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f35e5-139">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f35e5-139">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f35e5-140">Eseguire il comando seguente nel **Terminale integrato**:</span><span class="sxs-lookup"><span data-stu-id="f35e5-140">Run the following command in the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="f35e5-141">Nella directory *SignalRWebPack* viene creata un'app Web ASP.NET Core vuota destinata a .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f35e5-141">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="f35e5-142">Configurare Webpack e TypeScript</span><span class="sxs-lookup"><span data-stu-id="f35e5-142">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="f35e5-143">I passaggi seguenti consentono di configurare la conversione da TypeScript a JavaScript e di creare un bundle delle risorse sul lato client.</span><span class="sxs-lookup"><span data-stu-id="f35e5-143">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="f35e5-144">Eseguire il comando seguente nella radice del progetto per creare un file *package.json*:</span><span class="sxs-lookup"><span data-stu-id="f35e5-144">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="f35e5-145">Aggiungere la proprietà evidenziata al file *package.json*:</span><span class="sxs-lookup"><span data-stu-id="f35e5-145">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/3.x/snippets/package1.json?highlight=4)]

    <span data-ttu-id="f35e5-146">Impostando la proprietà `private` su `true` si evita la visualizzazione di avvisi di installazione del pacchetto nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="f35e5-146">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="f35e5-147">Installare i pacchetti npm necessari.</span><span class="sxs-lookup"><span data-stu-id="f35e5-147">Install the required npm packages.</span></span> <span data-ttu-id="f35e5-148">Eseguire il comando seguente dalla radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="f35e5-148">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="f35e5-149">Ecco alcuni dettagli del comando da tenere in considerazione:</span><span class="sxs-lookup"><span data-stu-id="f35e5-149">Some command details to note:</span></span>

    * <span data-ttu-id="f35e5-150">Un numero di versione segue il segno `@` per ogni nome di pacchetto.</span><span class="sxs-lookup"><span data-stu-id="f35e5-150">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="f35e5-151">npm installa queste versioni del pacchetto specifiche.</span><span class="sxs-lookup"><span data-stu-id="f35e5-151">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="f35e5-152">L'opzione `-E` disabilita il comportamento predefinito di npm, in base al quale gli operatori intervallo di [Versionamento Semantico](https://semver.org/) vengono scritti in *package.json*.</span><span class="sxs-lookup"><span data-stu-id="f35e5-152">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="f35e5-153">Ad esempio, viene usato `"webpack": "4.29.3"` invece di `"webpack": "^4.29.3"`.</span><span class="sxs-lookup"><span data-stu-id="f35e5-153">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="f35e5-154">Questa opzione impedisce che vengano eseguiti aggiornamenti non desiderati a versioni più recenti del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="f35e5-154">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="f35e5-155">Per informazioni dettagliate, vedere la documentazione ufficiale di [npm-install](https://docs.npmjs.com/cli/install).</span><span class="sxs-lookup"><span data-stu-id="f35e5-155">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="f35e5-156">Sostituire la proprietà `scripts` del file *package.json* con il frammento di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f35e5-156">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="f35e5-157">Ecco una spiegazione degli script:</span><span class="sxs-lookup"><span data-stu-id="f35e5-157">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="f35e5-158">`build`: crea un bundle delle risorse sul lato client in modalità di sviluppo e verifica la presenza di modifiche al file.</span><span class="sxs-lookup"><span data-stu-id="f35e5-158">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="f35e5-159">Questo controllo fa sì che il bundle venga rigenerato a ogni modifica del file di progetto.</span><span class="sxs-lookup"><span data-stu-id="f35e5-159">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="f35e5-160">L'opzione `mode` disabilita le ottimizzazioni di produzione, come l'eliminazione del codice non utilizzato e la minimizzazione.</span><span class="sxs-lookup"><span data-stu-id="f35e5-160">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="f35e5-161">Usare `build` solo in modalità di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="f35e5-161">Only use `build` in development.</span></span>
    * <span data-ttu-id="f35e5-162">`release`: crea un bundle delle risorse sul lato client in modalità di produzione.</span><span class="sxs-lookup"><span data-stu-id="f35e5-162">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="f35e5-163">`publish`: esegue lo script `release` per creare il bundle delle risorse sul lato client in modalità di produzione.</span><span class="sxs-lookup"><span data-stu-id="f35e5-163">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="f35e5-164">Chiama il comando [publish](/dotnet/core/tools/dotnet-publish) dell'interfaccia della riga di comando di .NET Core per pubblicare l'app.</span><span class="sxs-lookup"><span data-stu-id="f35e5-164">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="f35e5-165">Creare un file denominato *webpack.config.js* nella radice del progetto con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="f35e5-165">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/3.x/webpack.config.js)]

    <span data-ttu-id="f35e5-166">Il file precedente configura la compilazione di Webpack.</span><span class="sxs-lookup"><span data-stu-id="f35e5-166">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="f35e5-167">Ecco alcuni dettagli di configurazione del server da tenere in considerazione:</span><span class="sxs-lookup"><span data-stu-id="f35e5-167">Some configuration details to note:</span></span>

    * <span data-ttu-id="f35e5-168">La proprietà `output` esegue l'override del valore predefinito di *dist*.</span><span class="sxs-lookup"><span data-stu-id="f35e5-168">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="f35e5-169">Il bundle viene invece creato nella directory *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="f35e5-169">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="f35e5-170">La matrice `resolve.extensions` include *. js* per importare il codice JavaScript del client SignalR.</span><span class="sxs-lookup"><span data-stu-id="f35e5-170">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="f35e5-171">Creare una nuova directory *src* nella radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="f35e5-171">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="f35e5-172">La sua funzione è quella di archiviare le risorse lato client del progetto.</span><span class="sxs-lookup"><span data-stu-id="f35e5-172">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="f35e5-173">Creare *src/index.html* con il contenuto seguente.</span><span class="sxs-lookup"><span data-stu-id="f35e5-173">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/3.x/src/index.html)]

    <span data-ttu-id="f35e5-174">Il codice HTML precedente definisce il markup del boilerplate della home page.</span><span class="sxs-lookup"><span data-stu-id="f35e5-174">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="f35e5-175">Creare una nuova directory *src/css*.</span><span class="sxs-lookup"><span data-stu-id="f35e5-175">Create a new *src/css* directory.</span></span> <span data-ttu-id="f35e5-176">La sua funzione è quella di archiviare i file *.css* del progetto.</span><span class="sxs-lookup"><span data-stu-id="f35e5-176">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="f35e5-177">Creare *src/css/main.css* con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="f35e5-177">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/3.x/src/css/main.css)]

    <span data-ttu-id="f35e5-178">Il file *main.css* precedente definisce lo stile dell'app.</span><span class="sxs-lookup"><span data-stu-id="f35e5-178">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="f35e5-179">Creare *src/tsconfig.json* con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="f35e5-179">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/3.x/src/tsconfig.json)]

    <span data-ttu-id="f35e5-180">Il codice precedente configura il compilatore TypeScript in modo da produrre codice JavaScript compatibile con [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5.</span><span class="sxs-lookup"><span data-stu-id="f35e5-180">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="f35e5-181">Creare *src/index.ts* con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="f35e5-181">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="f35e5-182">Il compilatore TypeScript precedente recupera i riferimenti agli elementi DOM e collega due gestori eventi:</span><span class="sxs-lookup"><span data-stu-id="f35e5-182">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="f35e5-183">`keyup`: questo evento viene generato quando l'utente digita qualcosa nella casella di testo identificata come `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="f35e5-183">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="f35e5-184">La funzione `send` viene chiamata quando l'utente preme **INVIO**.</span><span class="sxs-lookup"><span data-stu-id="f35e5-184">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="f35e5-185">`click`: questo evento viene generato quando l'utente fa clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="f35e5-185">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="f35e5-186">Viene chiamata la funzione `send`.</span><span class="sxs-lookup"><span data-stu-id="f35e5-186">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="f35e5-187">Configurare l'app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f35e5-187">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="f35e5-188">Nel metodo `Startup.Configure` aggiungere chiamate a [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="f35e5-188">In the `Startup.Configure` method, add calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseStaticDefaultFiles&highlight=2-3)]

   <span data-ttu-id="f35e5-189">Il codice precedente consente al server di individuare e servire il file *index.html*, sia che l'utente immetta l'URL completo o l'URL radice dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="f35e5-189">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="f35e5-190">Alla fine del metodo `Startup.Configure`, eseguire il mapping di una route */Hub* all'hub `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="f35e5-190">At the end of the `Startup.Configure` method, map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="f35e5-191">Sostituire il codice che visualizza *Hello World!*</span><span class="sxs-lookup"><span data-stu-id="f35e5-191">Replace the code that displays *Hello World!*</span></span> <span data-ttu-id="f35e5-192">con la riga seguente:</span><span class="sxs-lookup"><span data-stu-id="f35e5-192">with the following line:</span></span> 

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseSignalR&highlight=3)]

1. <span data-ttu-id="f35e5-193">Nel metodo `Startup.ConfigureServices` chiamare [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_).</span><span class="sxs-lookup"><span data-stu-id="f35e5-193">In the `Startup.ConfigureServices` method, call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_).</span></span> <span data-ttu-id="f35e5-194">Aggiunge i servizi SignalR al progetto.</span><span class="sxs-lookup"><span data-stu-id="f35e5-194">It adds the SignalR services to your project.</span></span>

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="f35e5-195">Creare una nuova directory denominata *Hubs* nella radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="f35e5-195">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="f35e5-196">Lo scopo è archiviare l'hub SignalR, che verrà creato nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="f35e5-196">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="f35e5-197">Creare l'hub *Hubs/ChatHub.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f35e5-197">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="f35e5-198">Aggiungere il codice seguente all'inizio del file *Startup.cs* per risolvere il riferimento a `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="f35e5-198">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="f35e5-199">Abilitare la comunicazione tra client e server</span><span class="sxs-lookup"><span data-stu-id="f35e5-199">Enable client and server communication</span></span>

<span data-ttu-id="f35e5-200">L'app attualmente visualizza un semplice form per l'invio di messaggi.</span><span class="sxs-lookup"><span data-stu-id="f35e5-200">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="f35e5-201">Quando si prova a eseguire questa operazione, non accade nulla.</span><span class="sxs-lookup"><span data-stu-id="f35e5-201">Nothing happens when you try to do so.</span></span> <span data-ttu-id="f35e5-202">Il server è in ascolto di una route specifica, ma non esegue alcuna operazione con i messaggi inviati.</span><span class="sxs-lookup"><span data-stu-id="f35e5-202">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="f35e5-203">Eseguire il comando seguente nella radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="f35e5-203">Execute the following command at the project root:</span></span>

    ```console
    npm install @microsoft/signalr
    ```

    <span data-ttu-id="f35e5-204">Il comando precedente installa il [client diSignalR typescript](https://www.npmjs.com/package/@microsoft/signalr), che consente al client di inviare messaggi al server.</span><span class="sxs-lookup"><span data-stu-id="f35e5-204">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@microsoft/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="f35e5-205">Aggiungere il codice evidenziato al file *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="f35e5-205">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="f35e5-206">Il codice precedente supporta la ricezione di messaggi dal server.</span><span class="sxs-lookup"><span data-stu-id="f35e5-206">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="f35e5-207">La classe `HubConnectionBuilder` crea un nuovo compilatore per la configurazione della connessione al server.</span><span class="sxs-lookup"><span data-stu-id="f35e5-207">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="f35e5-208">La funzione `withUrl` configura l'URL dell'hub.</span><span class="sxs-lookup"><span data-stu-id="f35e5-208">The `withUrl` function configures the hub URL.</span></span>

    SignalR<span data-ttu-id="f35e5-209"> consente lo scambio di messaggi tra un client e un server.</span><span class="sxs-lookup"><span data-stu-id="f35e5-209"> enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="f35e5-210">Ogni messaggio ha un nome specifico.</span><span class="sxs-lookup"><span data-stu-id="f35e5-210">Each message has a specific name.</span></span> <span data-ttu-id="f35e5-211">Ad esempio, i messaggi con nome `messageReceived` eseguono la logica responsabile della visualizzazione del nuovo messaggio nell'area dei messaggi.</span><span class="sxs-lookup"><span data-stu-id="f35e5-211">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="f35e5-212">L'ascolto di un messaggio specifico può essere eseguito tramite la funzione `on`.</span><span class="sxs-lookup"><span data-stu-id="f35e5-212">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="f35e5-213">È possibile restare in ascolto di un numero indefinito di nomi di messaggio.</span><span class="sxs-lookup"><span data-stu-id="f35e5-213">You can listen to any number of message names.</span></span> <span data-ttu-id="f35e5-214">Si può anche passare parametri al messaggio, come il nome dell'autore e il contenuto del messaggio ricevuto.</span><span class="sxs-lookup"><span data-stu-id="f35e5-214">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="f35e5-215">Quando il client riceve un messaggio, viene creato un nuovo elemento `div` con il nome dell'autore e il contenuto del messaggio nell'attributo `innerHTML`.</span><span class="sxs-lookup"><span data-stu-id="f35e5-215">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="f35e5-216">Viene aggiunto all'elemento `div` principale che visualizza i messaggi.</span><span class="sxs-lookup"><span data-stu-id="f35e5-216">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="f35e5-217">Ora che il client può ricevere messaggi, configurarlo anche per l'invio.</span><span class="sxs-lookup"><span data-stu-id="f35e5-217">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="f35e5-218">Aggiungere il codice evidenziato al file *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="f35e5-218">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/src/index.ts?highlight=34-35)]

    <span data-ttu-id="f35e5-219">Per inviare un messaggio attraverso la connessione WebSockets è necessario chiamare il metodo `send`.</span><span class="sxs-lookup"><span data-stu-id="f35e5-219">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="f35e5-220">Il primo parametro del metodo è il nome del messaggio.</span><span class="sxs-lookup"><span data-stu-id="f35e5-220">The method's first parameter is the message name.</span></span> <span data-ttu-id="f35e5-221">I dati del messaggio sono rappresentati dagli altri parametri.</span><span class="sxs-lookup"><span data-stu-id="f35e5-221">The message data inhabits the other parameters.</span></span> <span data-ttu-id="f35e5-222">In questo esempio un messaggio identificato come `newMessage` viene inviato al server.</span><span class="sxs-lookup"><span data-stu-id="f35e5-222">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="f35e5-223">Il messaggio è costituito dal nome utente e dall'input dell'utente in una casella di testo.</span><span class="sxs-lookup"><span data-stu-id="f35e5-223">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="f35e5-224">Se l'invio riesce, il valore della casella di testo viene cancellato.</span><span class="sxs-lookup"><span data-stu-id="f35e5-224">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="f35e5-225">Aggiungere il metodo evidenziato alla classe `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="f35e5-225">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="f35e5-226">Il codice precedente trasmette i messaggi ricevuti a tutti gli utenti connessi dopo che il server li riceve.</span><span class="sxs-lookup"><span data-stu-id="f35e5-226">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="f35e5-227">Non è necessario avere un metodo generico `on` per ricevere tutti i messaggi.</span><span class="sxs-lookup"><span data-stu-id="f35e5-227">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="f35e5-228">È sufficiente un metodo denominato dopo il nome del messaggio.</span><span class="sxs-lookup"><span data-stu-id="f35e5-228">A method named after the message name suffices.</span></span>

    <span data-ttu-id="f35e5-229">In questo esempio il client TypeScript invia un messaggio identificato come `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="f35e5-229">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="f35e5-230">Il metodo C# `NewMessage` si aspetta i dati inviati dal client.</span><span class="sxs-lookup"><span data-stu-id="f35e5-230">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="f35e5-231">Viene eseguita una chiamata al metodo [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) su [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="f35e5-231">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="f35e5-232">I messaggi ricevuti vengono inviati a tutti i client connessi all'hub.</span><span class="sxs-lookup"><span data-stu-id="f35e5-232">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="f35e5-233">Eseguire il test dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="f35e5-233">Test the app</span></span>

<span data-ttu-id="f35e5-234">Per verificare che l'app funzioni, eseguire la procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="f35e5-234">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f35e5-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f35e5-235">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="f35e5-236">Eseguire Webpack in modalità *versione*.</span><span class="sxs-lookup"><span data-stu-id="f35e5-236">Run Webpack in *release* mode.</span></span> <span data-ttu-id="f35e5-237">Nella finestra **Console di gestione pacchetti** eseguire il comando seguente nella radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="f35e5-237">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="f35e5-238">Se non ci si trova nella directory radice del progetto, immettere `cd SignalRWebPack` prima di immettere il comando.</span><span class="sxs-lookup"><span data-stu-id="f35e5-238">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="f35e5-239">Selezionare **Debug** > **Avvia senza eseguire il debug** per avviare l'app in un browser senza collegare il debugger.</span><span class="sxs-lookup"><span data-stu-id="f35e5-239">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="f35e5-240">Il file *wwwroot/index.html* viene servito all'indirizzo `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="f35e5-240">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

   <span data-ttu-id="f35e5-241">Se vengono visualizzati errori di compilazione, provare a chiudere e riaprire la soluzione.</span><span class="sxs-lookup"><span data-stu-id="f35e5-241">If you get compile errors, try closing and reopening the solution.</span></span> 

1. <span data-ttu-id="f35e5-242">Aprire un'altra istanza del browser (qualsiasi browser).</span><span class="sxs-lookup"><span data-stu-id="f35e5-242">Open another browser instance (any browser).</span></span> <span data-ttu-id="f35e5-243">Incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="f35e5-243">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="f35e5-244">Scegliere un browser, digitare qualcosa nella casella di testo **Messaggio** e fare clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="f35e5-244">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="f35e5-245">Il nome utente e il messaggio univoci vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="f35e5-245">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f35e5-246">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f35e5-246">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="f35e5-247">Eseguire Webpack in modalità *versione* eseguendo il comando seguente nella radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="f35e5-247">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="f35e5-248">Compilare ed eseguire l'app eseguendo il comando seguente nella radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="f35e5-248">Build and run the app by executing the following command in the project root:</span></span>

    ```dotnetcli
    dotnet run
    ```

    <span data-ttu-id="f35e5-249">Il server Web avvia l'app e la rende disponibile su localhost.</span><span class="sxs-lookup"><span data-stu-id="f35e5-249">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="f35e5-250">Aprire un browser all'indirizzo `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="f35e5-250">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="f35e5-251">Il file *wwwroot/index.html* viene servito.</span><span class="sxs-lookup"><span data-stu-id="f35e5-251">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="f35e5-252">Copiare l'URL dalla barra dell'indirizzo.</span><span class="sxs-lookup"><span data-stu-id="f35e5-252">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="f35e5-253">Aprire un'altra istanza del browser (qualsiasi browser).</span><span class="sxs-lookup"><span data-stu-id="f35e5-253">Open another browser instance (any browser).</span></span> <span data-ttu-id="f35e5-254">Incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="f35e5-254">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="f35e5-255">Scegliere un browser, digitare qualcosa nella casella di testo **Messaggio** e fare clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="f35e5-255">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="f35e5-256">Il nome utente e il messaggio univoci vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="f35e5-256">The unique user name and message are displayed on both pages instantly.</span></span>

---

![Messaggio visualizzato in entrambe le finestre del browser](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f35e5-258">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f35e5-258">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f35e5-259">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) con il carico di lavoro **Sviluppo ASP.NET e Web**</span><span class="sxs-lookup"><span data-stu-id="f35e5-259">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="f35e5-260">.NET Core SDK 2.2 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="f35e5-260">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="f35e5-261">[Node.js](https://nodejs.org/) con [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="f35e5-261">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f35e5-262">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f35e5-262">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="f35e5-263">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f35e5-263">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="f35e5-264">.NET Core SDK 2.2 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="f35e5-264">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="f35e5-265">C# per Visual Studio Code 1.17.1 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="f35e5-265">C# for Visual Studio Code version 1.17.1 or later</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="f35e5-266">[Node.js](https://nodejs.org/) con [npm](https://www.npmjs.com/)</span><span class="sxs-lookup"><span data-stu-id="f35e5-266">[Node.js](https://nodejs.org/) with [npm](https://www.npmjs.com/)</span></span>

---

## <a name="create-the-aspnet-core-web-app"></a><span data-ttu-id="f35e5-267">Creare l'app Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f35e5-267">Create the ASP.NET Core web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f35e5-268">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f35e5-268">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f35e5-269">Configurare Visual Studio in modo che cerchi npm nella variabile di ambiente *PATH*.</span><span class="sxs-lookup"><span data-stu-id="f35e5-269">Configure Visual Studio to look for npm in the *PATH* environment variable.</span></span> <span data-ttu-id="f35e5-270">Per impostazione predefinita, Visual Studio usa la versione di npm trovata nella directory di installazione.</span><span class="sxs-lookup"><span data-stu-id="f35e5-270">By default, Visual Studio uses the version of npm found in its installation directory.</span></span> <span data-ttu-id="f35e5-271">Seguire queste istruzioni in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="f35e5-271">Follow these instructions in Visual Studio:</span></span>

1. <span data-ttu-id="f35e5-272">Passare a **strumenti** > **Opzioni** > **progetti e soluzioni** > **Gestione pacchetti Web** > **strumenti Web esterni**.</span><span class="sxs-lookup"><span data-stu-id="f35e5-272">Navigate to **Tools** > **Options** > **Projects and Solutions** > **Web Package Management** > **External Web Tools**.</span></span>
1. <span data-ttu-id="f35e5-273">Selezionare la voce *$(PATH)* dall'elenco.</span><span class="sxs-lookup"><span data-stu-id="f35e5-273">Select the *$(PATH)* entry from the list.</span></span> <span data-ttu-id="f35e5-274">Fare clic sulla freccia in su per spostare la voce nella seconda posizione nell'elenco.</span><span class="sxs-lookup"><span data-stu-id="f35e5-274">Click the up arrow to move the entry to the second position in the list.</span></span>

    ![Configurazione di Visual Studio](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

<span data-ttu-id="f35e5-276">La configurazione di Visual Studio è completata.</span><span class="sxs-lookup"><span data-stu-id="f35e5-276">Visual Studio configuration is completed.</span></span> <span data-ttu-id="f35e5-277">A questo punto è possibile creare il progetto</span><span class="sxs-lookup"><span data-stu-id="f35e5-277">It's time to create the project.</span></span>

1. <span data-ttu-id="f35e5-278">Utilizzare l'opzione di menu **File** > **nuovo** > **progetto** e scegliere il modello **applicazione Web ASP.NET Core** .</span><span class="sxs-lookup"><span data-stu-id="f35e5-278">Use the **File** > **New** > **Project** menu option and choose the **ASP.NET Core Web Application** template.</span></span>
1. <span data-ttu-id="f35e5-279">Assegnare al progetto il nome *SignalRWebPack*e selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="f35e5-279">Name the project *SignalRWebPack*, and select **Create**.</span></span>
1. <span data-ttu-id="f35e5-280">Selezionare *.NET Core* nell'elenco a discesa del framework di destinazione e quindi selezionare *ASP.NET Core 2.2* nell'elenco a discesa del selettore del framework.</span><span class="sxs-lookup"><span data-stu-id="f35e5-280">Select *.NET Core* from the target framework drop-down, and select *ASP.NET Core 2.2* from the framework selector drop-down.</span></span> <span data-ttu-id="f35e5-281">Selezionare il modello **vuoto** e selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="f35e5-281">Select the **Empty** template, and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f35e5-282">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f35e5-282">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f35e5-283">Eseguire il comando seguente nel **Terminale integrato**:</span><span class="sxs-lookup"><span data-stu-id="f35e5-283">Run the following command in the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet new web -o SignalRWebPack
```

<span data-ttu-id="f35e5-284">Nella directory *SignalRWebPack* viene creata un'app Web ASP.NET Core vuota destinata a .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f35e5-284">An empty ASP.NET Core web app, targeting .NET Core, is created in a *SignalRWebPack* directory.</span></span>

---

## <a name="configure-webpack-and-typescript"></a><span data-ttu-id="f35e5-285">Configurare Webpack e TypeScript</span><span class="sxs-lookup"><span data-stu-id="f35e5-285">Configure Webpack and TypeScript</span></span>

<span data-ttu-id="f35e5-286">I passaggi seguenti consentono di configurare la conversione da TypeScript a JavaScript e di creare un bundle delle risorse sul lato client.</span><span class="sxs-lookup"><span data-stu-id="f35e5-286">The following steps configure the conversion of TypeScript to JavaScript and the bundling of client-side resources.</span></span>

1. <span data-ttu-id="f35e5-287">Eseguire il comando seguente nella radice del progetto per creare un file *package.json*:</span><span class="sxs-lookup"><span data-stu-id="f35e5-287">Execute the following command in the project root to create a *package.json* file:</span></span>

    ```console
    npm init -y
    ```

1. <span data-ttu-id="f35e5-288">Aggiungere la proprietà evidenziata al file *package.json*:</span><span class="sxs-lookup"><span data-stu-id="f35e5-288">Add the highlighted property to the *package.json* file:</span></span>

    [!code-json[package.json](signalr-typescript-webpack/sample/2.x/snippets/package1.json?highlight=4)]

    <span data-ttu-id="f35e5-289">Impostando la proprietà `private` su `true` si evita la visualizzazione di avvisi di installazione del pacchetto nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="f35e5-289">Setting the `private` property to `true` prevents package installation warnings in the next step.</span></span>

1. <span data-ttu-id="f35e5-290">Installare i pacchetti npm necessari.</span><span class="sxs-lookup"><span data-stu-id="f35e5-290">Install the required npm packages.</span></span> <span data-ttu-id="f35e5-291">Eseguire il comando seguente dalla radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="f35e5-291">Execute the following command from the project root:</span></span>

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    <span data-ttu-id="f35e5-292">Ecco alcuni dettagli del comando da tenere in considerazione:</span><span class="sxs-lookup"><span data-stu-id="f35e5-292">Some command details to note:</span></span>

    * <span data-ttu-id="f35e5-293">Un numero di versione segue il segno `@` per ogni nome di pacchetto.</span><span class="sxs-lookup"><span data-stu-id="f35e5-293">A version number follows the `@` sign for each package name.</span></span> <span data-ttu-id="f35e5-294">npm installa queste versioni del pacchetto specifiche.</span><span class="sxs-lookup"><span data-stu-id="f35e5-294">npm installs those specific package versions.</span></span>
    * <span data-ttu-id="f35e5-295">L'opzione `-E` disabilita il comportamento predefinito di npm, in base al quale gli operatori intervallo di [Versionamento Semantico](https://semver.org/) vengono scritti in *package.json*.</span><span class="sxs-lookup"><span data-stu-id="f35e5-295">The `-E` option disables npm's default behavior of writing [semantic versioning](https://semver.org/) range operators to *package.json*.</span></span> <span data-ttu-id="f35e5-296">Ad esempio, viene usato `"webpack": "4.29.3"` invece di `"webpack": "^4.29.3"`.</span><span class="sxs-lookup"><span data-stu-id="f35e5-296">For example, `"webpack": "4.29.3"` is used instead of `"webpack": "^4.29.3"`.</span></span> <span data-ttu-id="f35e5-297">Questa opzione impedisce che vengano eseguiti aggiornamenti non desiderati a versioni più recenti del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="f35e5-297">This option prevents unintended upgrades to newer package versions.</span></span>

    <span data-ttu-id="f35e5-298">Per informazioni dettagliate, vedere la documentazione ufficiale di [npm-install](https://docs.npmjs.com/cli/install).</span><span class="sxs-lookup"><span data-stu-id="f35e5-298">See the official [npm-install](https://docs.npmjs.com/cli/install) docs for more detail.</span></span>

1. <span data-ttu-id="f35e5-299">Sostituire la proprietà `scripts` del file *package.json* con il frammento di codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f35e5-299">Replace the `scripts` property of the *package.json* file with the following snippet:</span></span>

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    <span data-ttu-id="f35e5-300">Ecco una spiegazione degli script:</span><span class="sxs-lookup"><span data-stu-id="f35e5-300">Some explanation of the scripts:</span></span>

    * <span data-ttu-id="f35e5-301">`build`: crea un bundle delle risorse sul lato client in modalità di sviluppo e verifica la presenza di modifiche al file.</span><span class="sxs-lookup"><span data-stu-id="f35e5-301">`build`: Bundles your client-side resources in development mode and watches for file changes.</span></span> <span data-ttu-id="f35e5-302">Questo controllo fa sì che il bundle venga rigenerato a ogni modifica del file di progetto.</span><span class="sxs-lookup"><span data-stu-id="f35e5-302">The file watcher causes the bundle to regenerate each time a project file changes.</span></span> <span data-ttu-id="f35e5-303">L'opzione `mode` disabilita le ottimizzazioni di produzione, come l'eliminazione del codice non utilizzato e la minimizzazione.</span><span class="sxs-lookup"><span data-stu-id="f35e5-303">The `mode` option disables production optimizations, such as tree shaking and minification.</span></span> <span data-ttu-id="f35e5-304">Usare `build` solo in modalità di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="f35e5-304">Only use `build` in development.</span></span>
    * <span data-ttu-id="f35e5-305">`release`: crea un bundle delle risorse sul lato client in modalità di produzione.</span><span class="sxs-lookup"><span data-stu-id="f35e5-305">`release`: Bundles your client-side resources in production mode.</span></span>
    * <span data-ttu-id="f35e5-306">`publish`: esegue lo script `release` per creare il bundle delle risorse sul lato client in modalità di produzione.</span><span class="sxs-lookup"><span data-stu-id="f35e5-306">`publish`: Runs the `release` script to bundle the client-side resources in production mode.</span></span> <span data-ttu-id="f35e5-307">Chiama il comando [publish](/dotnet/core/tools/dotnet-publish) dell'interfaccia della riga di comando di .NET Core per pubblicare l'app.</span><span class="sxs-lookup"><span data-stu-id="f35e5-307">It calls the .NET Core CLI's [publish](/dotnet/core/tools/dotnet-publish) command to publish the app.</span></span>

1. <span data-ttu-id="f35e5-308">Creare un file denominato *webpack.config.js* nella radice del progetto con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="f35e5-308">Create a file named *webpack.config.js*, in the project root, with the following content:</span></span>

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/2.x/webpack.config.js)]

    <span data-ttu-id="f35e5-309">Il file precedente configura la compilazione di Webpack.</span><span class="sxs-lookup"><span data-stu-id="f35e5-309">The preceding file configures the Webpack compilation.</span></span> <span data-ttu-id="f35e5-310">Ecco alcuni dettagli di configurazione del server da tenere in considerazione:</span><span class="sxs-lookup"><span data-stu-id="f35e5-310">Some configuration details to note:</span></span>

    * <span data-ttu-id="f35e5-311">La proprietà `output` esegue l'override del valore predefinito di *dist*.</span><span class="sxs-lookup"><span data-stu-id="f35e5-311">The `output` property overrides the default value of *dist*.</span></span> <span data-ttu-id="f35e5-312">Il bundle viene invece creato nella directory *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="f35e5-312">The bundle is instead emitted in the *wwwroot* directory.</span></span>
    * <span data-ttu-id="f35e5-313">La matrice `resolve.extensions` include *. js* per importare il codice JavaScript del client SignalR.</span><span class="sxs-lookup"><span data-stu-id="f35e5-313">The `resolve.extensions` array includes *.js* to import the SignalR client JavaScript.</span></span>

1. <span data-ttu-id="f35e5-314">Creare una nuova directory *src* nella radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="f35e5-314">Create a new *src* directory in the project root.</span></span> <span data-ttu-id="f35e5-315">La sua funzione è quella di archiviare le risorse lato client del progetto.</span><span class="sxs-lookup"><span data-stu-id="f35e5-315">Its purpose is to store the project's client-side assets.</span></span>

1. <span data-ttu-id="f35e5-316">Creare *src/index.html* con il contenuto seguente.</span><span class="sxs-lookup"><span data-stu-id="f35e5-316">Create *src/index.html* with the following content.</span></span>

    [!code-html[index.html](signalr-typescript-webpack/sample/2.x/src/index.html)]

    <span data-ttu-id="f35e5-317">Il codice HTML precedente definisce il markup del boilerplate della home page.</span><span class="sxs-lookup"><span data-stu-id="f35e5-317">The preceding HTML defines the homepage's boilerplate markup.</span></span>

1. <span data-ttu-id="f35e5-318">Creare una nuova directory *src/css*.</span><span class="sxs-lookup"><span data-stu-id="f35e5-318">Create a new *src/css* directory.</span></span> <span data-ttu-id="f35e5-319">La sua funzione è quella di archiviare i file *.css* del progetto.</span><span class="sxs-lookup"><span data-stu-id="f35e5-319">Its purpose is to store the project's *.css* files.</span></span>

1. <span data-ttu-id="f35e5-320">Creare *src/css/main.css* con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="f35e5-320">Create *src/css/main.css* with the following content:</span></span>

    [!code-css[main.css](signalr-typescript-webpack/sample/2.x/src/css/main.css)]

    <span data-ttu-id="f35e5-321">Il file *main.css* precedente definisce lo stile dell'app.</span><span class="sxs-lookup"><span data-stu-id="f35e5-321">The preceding *main.css* file styles the app.</span></span>

1. <span data-ttu-id="f35e5-322">Creare *src/tsconfig.json* con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="f35e5-322">Create *src/tsconfig.json* with the following content:</span></span>

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/2.x/src/tsconfig.json)]

    <span data-ttu-id="f35e5-323">Il codice precedente configura il compilatore TypeScript in modo da produrre codice JavaScript compatibile con [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5.</span><span class="sxs-lookup"><span data-stu-id="f35e5-323">The preceding code configures the TypeScript compiler to produce [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5-compatible JavaScript.</span></span>

1. <span data-ttu-id="f35e5-324">Creare *src/index.ts* con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="f35e5-324">Create *src/index.ts* with the following content:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    <span data-ttu-id="f35e5-325">Il compilatore TypeScript precedente recupera i riferimenti agli elementi DOM e collega due gestori eventi:</span><span class="sxs-lookup"><span data-stu-id="f35e5-325">The preceding TypeScript retrieves references to DOM elements and attaches two event handlers:</span></span>

    * <span data-ttu-id="f35e5-326">`keyup`: questo evento viene generato quando l'utente digita qualcosa nella casella di testo identificata come `tbMessage`.</span><span class="sxs-lookup"><span data-stu-id="f35e5-326">`keyup`: This event fires when the user types something in the textbox identified as `tbMessage`.</span></span> <span data-ttu-id="f35e5-327">La funzione `send` viene chiamata quando l'utente preme **INVIO**.</span><span class="sxs-lookup"><span data-stu-id="f35e5-327">The `send` function is called when the user presses the **Enter** key.</span></span>
    * <span data-ttu-id="f35e5-328">`click`: questo evento viene generato quando l'utente fa clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="f35e5-328">`click`: This event fires when the user clicks the **Send** button.</span></span> <span data-ttu-id="f35e5-329">Viene chiamata la funzione `send`.</span><span class="sxs-lookup"><span data-stu-id="f35e5-329">The `send` function is called.</span></span>

## <a name="configure-the-aspnet-core-app"></a><span data-ttu-id="f35e5-330">Configurare l'app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f35e5-330">Configure the ASP.NET Core app</span></span>

1. <span data-ttu-id="f35e5-331">Il codice fornito nel metodo `Startup.Configure` visualizza *Hello World!* .</span><span class="sxs-lookup"><span data-stu-id="f35e5-331">The code provided in the `Startup.Configure` method displays *Hello World!*.</span></span> <span data-ttu-id="f35e5-332">Sostituire la chiamata del metodo `app.Run` con chiamate a [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="f35e5-332">Replace the `app.Run` method call with calls to [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    <span data-ttu-id="f35e5-333">Il codice precedente consente al server di individuare e servire il file *index.html*, sia che l'utente immetta l'URL completo o l'URL radice dell'app Web.</span><span class="sxs-lookup"><span data-stu-id="f35e5-333">The preceding code allows the server to locate and serve the *index.html* file, whether the user enters its full URL or the root URL of the web app.</span></span>

1. <span data-ttu-id="f35e5-334">Chiamare [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) nel metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f35e5-334">Call [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="f35e5-335">Aggiunge i servizi SignalR al progetto.</span><span class="sxs-lookup"><span data-stu-id="f35e5-335">It adds the SignalR services to your project.</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_AddSignalR)]

1. <span data-ttu-id="f35e5-336">Eseguire il mapping di una route */hub* all'hub `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="f35e5-336">Map a */hub* route to the `ChatHub` hub.</span></span> <span data-ttu-id="f35e5-337">Aggiungere le righe seguenti alla fine del metodo `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="f35e5-337">Add the following lines at the end of the `Startup.Configure` method:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseSignalR)]

1. <span data-ttu-id="f35e5-338">Creare una nuova directory denominata *Hubs* nella radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="f35e5-338">Create a new directory, called *Hubs*, in the project root.</span></span> <span data-ttu-id="f35e5-339">Lo scopo è archiviare l'hub SignalR, che verrà creato nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="f35e5-339">Its purpose is to store the SignalR hub, which is created in the next step.</span></span>

1. <span data-ttu-id="f35e5-340">Creare l'hub *Hubs/ChatHub.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f35e5-340">Create hub *Hubs/ChatHub.cs* with the following code:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. <span data-ttu-id="f35e5-341">Aggiungere il codice seguente all'inizio del file *Startup.cs* per risolvere il riferimento a `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="f35e5-341">Add the following code at the top of the *Startup.cs* file to resolve the `ChatHub` reference:</span></span>

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a><span data-ttu-id="f35e5-342">Abilitare la comunicazione tra client e server</span><span class="sxs-lookup"><span data-stu-id="f35e5-342">Enable client and server communication</span></span>

<span data-ttu-id="f35e5-343">L'app attualmente visualizza un semplice form per l'invio di messaggi.</span><span class="sxs-lookup"><span data-stu-id="f35e5-343">The app currently displays a simple form to send messages.</span></span> <span data-ttu-id="f35e5-344">Quando si prova a eseguire questa operazione, non accade nulla.</span><span class="sxs-lookup"><span data-stu-id="f35e5-344">Nothing happens when you try to do so.</span></span> <span data-ttu-id="f35e5-345">Il server è in ascolto di una route specifica, ma non esegue alcuna operazione con i messaggi inviati.</span><span class="sxs-lookup"><span data-stu-id="f35e5-345">The server is listening to a specific route but does nothing with sent messages.</span></span>

1. <span data-ttu-id="f35e5-346">Eseguire il comando seguente nella radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="f35e5-346">Execute the following command at the project root:</span></span>

    ```console
    npm install @microsoft/signalr
    ```

    <span data-ttu-id="f35e5-347">Il comando precedente installa il [client diSignalR typescript](https://www.npmjs.com/package/@microsoft/signalr), che consente al client di inviare messaggi al server.</span><span class="sxs-lookup"><span data-stu-id="f35e5-347">The preceding command installs the [SignalR TypeScript client](https://www.npmjs.com/package/@microsoft/signalr), which allows the client to send messages to the server.</span></span>

1. <span data-ttu-id="f35e5-348">Aggiungere il codice evidenziato al file *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="f35e5-348">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    <span data-ttu-id="f35e5-349">Il codice precedente supporta la ricezione di messaggi dal server.</span><span class="sxs-lookup"><span data-stu-id="f35e5-349">The preceding code supports receiving messages from the server.</span></span> <span data-ttu-id="f35e5-350">La classe `HubConnectionBuilder` crea un nuovo compilatore per la configurazione della connessione al server.</span><span class="sxs-lookup"><span data-stu-id="f35e5-350">The `HubConnectionBuilder` class creates a new builder for configuring the server connection.</span></span> <span data-ttu-id="f35e5-351">La funzione `withUrl` configura l'URL dell'hub.</span><span class="sxs-lookup"><span data-stu-id="f35e5-351">The `withUrl` function configures the hub URL.</span></span>

    SignalR<span data-ttu-id="f35e5-352"> consente lo scambio di messaggi tra un client e un server.</span><span class="sxs-lookup"><span data-stu-id="f35e5-352"> enables the exchange of messages between a client and a server.</span></span> <span data-ttu-id="f35e5-353">Ogni messaggio ha un nome specifico.</span><span class="sxs-lookup"><span data-stu-id="f35e5-353">Each message has a specific name.</span></span> <span data-ttu-id="f35e5-354">Ad esempio, i messaggi con nome `messageReceived` eseguono la logica responsabile della visualizzazione del nuovo messaggio nell'area dei messaggi.</span><span class="sxs-lookup"><span data-stu-id="f35e5-354">For example, you can have messages with the name `messageReceived` that execute the logic responsible for displaying the new message in the messages zone.</span></span> <span data-ttu-id="f35e5-355">L'ascolto di un messaggio specifico può essere eseguito tramite la funzione `on`.</span><span class="sxs-lookup"><span data-stu-id="f35e5-355">Listening to a specific message can be done via the `on` function.</span></span> <span data-ttu-id="f35e5-356">È possibile restare in ascolto di un numero indefinito di nomi di messaggio.</span><span class="sxs-lookup"><span data-stu-id="f35e5-356">You can listen to any number of message names.</span></span> <span data-ttu-id="f35e5-357">Si può anche passare parametri al messaggio, come il nome dell'autore e il contenuto del messaggio ricevuto.</span><span class="sxs-lookup"><span data-stu-id="f35e5-357">It's also possible to pass parameters to the message, such as the author's name and the content of the message received.</span></span> <span data-ttu-id="f35e5-358">Quando il client riceve un messaggio, viene creato un nuovo elemento `div` con il nome dell'autore e il contenuto del messaggio nell'attributo `innerHTML`.</span><span class="sxs-lookup"><span data-stu-id="f35e5-358">Once the client receives a message, a new `div` element is created with the author's name and the message content in its `innerHTML` attribute.</span></span> <span data-ttu-id="f35e5-359">Viene aggiunto all'elemento `div` principale che visualizza i messaggi.</span><span class="sxs-lookup"><span data-stu-id="f35e5-359">It's added to the main `div` element displaying the messages.</span></span>

1. <span data-ttu-id="f35e5-360">Ora che il client può ricevere messaggi, configurarlo anche per l'invio.</span><span class="sxs-lookup"><span data-stu-id="f35e5-360">Now that the client can receive a message, configure it to send messages.</span></span> <span data-ttu-id="f35e5-361">Aggiungere il codice evidenziato al file *src/index.ts*:</span><span class="sxs-lookup"><span data-stu-id="f35e5-361">Add the highlighted code to the *src/index.ts* file:</span></span>

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/src/index.ts?highlight=34-35)]

    <span data-ttu-id="f35e5-362">Per inviare un messaggio attraverso la connessione WebSockets è necessario chiamare il metodo `send`.</span><span class="sxs-lookup"><span data-stu-id="f35e5-362">Sending a message through the WebSockets connection requires calling the `send` method.</span></span> <span data-ttu-id="f35e5-363">Il primo parametro del metodo è il nome del messaggio.</span><span class="sxs-lookup"><span data-stu-id="f35e5-363">The method's first parameter is the message name.</span></span> <span data-ttu-id="f35e5-364">I dati del messaggio sono rappresentati dagli altri parametri.</span><span class="sxs-lookup"><span data-stu-id="f35e5-364">The message data inhabits the other parameters.</span></span> <span data-ttu-id="f35e5-365">In questo esempio un messaggio identificato come `newMessage` viene inviato al server.</span><span class="sxs-lookup"><span data-stu-id="f35e5-365">In this example, a message identified as `newMessage` is sent to the server.</span></span> <span data-ttu-id="f35e5-366">Il messaggio è costituito dal nome utente e dall'input dell'utente in una casella di testo.</span><span class="sxs-lookup"><span data-stu-id="f35e5-366">The message consists of the username and the user input from a text box.</span></span> <span data-ttu-id="f35e5-367">Se l'invio riesce, il valore della casella di testo viene cancellato.</span><span class="sxs-lookup"><span data-stu-id="f35e5-367">If the send works, the text box value is cleared.</span></span>

1. <span data-ttu-id="f35e5-368">Aggiungere il metodo evidenziato alla classe `ChatHub`:</span><span class="sxs-lookup"><span data-stu-id="f35e5-368">Add the highlighted method to the `ChatHub` class:</span></span>

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/Hubs/ChatHub.cs?highlight=8-11)]

    <span data-ttu-id="f35e5-369">Il codice precedente trasmette i messaggi ricevuti a tutti gli utenti connessi dopo che il server li riceve.</span><span class="sxs-lookup"><span data-stu-id="f35e5-369">The preceding code broadcasts received messages to all connected users once the server receives them.</span></span> <span data-ttu-id="f35e5-370">Non è necessario avere un metodo generico `on` per ricevere tutti i messaggi.</span><span class="sxs-lookup"><span data-stu-id="f35e5-370">It's unnecessary to have a generic `on` method to receive all the messages.</span></span> <span data-ttu-id="f35e5-371">È sufficiente un metodo denominato dopo il nome del messaggio.</span><span class="sxs-lookup"><span data-stu-id="f35e5-371">A method named after the message name suffices.</span></span>

    <span data-ttu-id="f35e5-372">In questo esempio il client TypeScript invia un messaggio identificato come `newMessage`.</span><span class="sxs-lookup"><span data-stu-id="f35e5-372">In this example, the TypeScript client sends a message identified as `newMessage`.</span></span> <span data-ttu-id="f35e5-373">Il metodo C# `NewMessage` si aspetta i dati inviati dal client.</span><span class="sxs-lookup"><span data-stu-id="f35e5-373">The C# `NewMessage` method expects the data sent by the client.</span></span> <span data-ttu-id="f35e5-374">Viene eseguita una chiamata al metodo [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) su [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span><span class="sxs-lookup"><span data-stu-id="f35e5-374">A call is made to the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method on [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all).</span></span> <span data-ttu-id="f35e5-375">I messaggi ricevuti vengono inviati a tutti i client connessi all'hub.</span><span class="sxs-lookup"><span data-stu-id="f35e5-375">The received messages are sent to all clients connected to the hub.</span></span>

## <a name="test-the-app"></a><span data-ttu-id="f35e5-376">Eseguire il test dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="f35e5-376">Test the app</span></span>

<span data-ttu-id="f35e5-377">Per verificare che l'app funzioni, eseguire la procedura seguente.</span><span class="sxs-lookup"><span data-stu-id="f35e5-377">Confirm that the app works with the following steps.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f35e5-378">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f35e5-378">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="f35e5-379">Eseguire Webpack in modalità *versione*.</span><span class="sxs-lookup"><span data-stu-id="f35e5-379">Run Webpack in *release* mode.</span></span> <span data-ttu-id="f35e5-380">Nella finestra **Console di gestione pacchetti** eseguire il comando seguente nella radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="f35e5-380">Using the **Package Manager Console** window, execute the following command in the project root.</span></span> <span data-ttu-id="f35e5-381">Se non ci si trova nella directory radice del progetto, immettere `cd SignalRWebPack` prima di immettere il comando.</span><span class="sxs-lookup"><span data-stu-id="f35e5-381">If you are not in the project root, enter `cd SignalRWebPack` before entering the command.</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="f35e5-382">Selezionare **Debug** > **Avvia senza eseguire il debug** per avviare l'app in un browser senza collegare il debugger.</span><span class="sxs-lookup"><span data-stu-id="f35e5-382">Select **Debug** > **Start without debugging** to launch the app in a browser without attaching the debugger.</span></span> <span data-ttu-id="f35e5-383">Il file *wwwroot/index.html* viene servito all'indirizzo `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="f35e5-383">The *wwwroot/index.html* file is served at `http://localhost:<port_number>`.</span></span>

1. <span data-ttu-id="f35e5-384">Aprire un'altra istanza del browser (qualsiasi browser).</span><span class="sxs-lookup"><span data-stu-id="f35e5-384">Open another browser instance (any browser).</span></span> <span data-ttu-id="f35e5-385">Incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="f35e5-385">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="f35e5-386">Scegliere un browser, digitare qualcosa nella casella di testo **Messaggio** e fare clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="f35e5-386">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="f35e5-387">Il nome utente e il messaggio univoci vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="f35e5-387">The unique user name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f35e5-388">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f35e5-388">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="f35e5-389">Eseguire Webpack in modalità *versione* eseguendo il comando seguente nella radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="f35e5-389">Run Webpack in *release* mode by executing the following command in the project root:</span></span>

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. <span data-ttu-id="f35e5-390">Compilare ed eseguire l'app eseguendo il comando seguente nella radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="f35e5-390">Build and run the app by executing the following command in the project root:</span></span>

    ```dotnetcli
    dotnet run
    ```

    <span data-ttu-id="f35e5-391">Il server Web avvia l'app e la rende disponibile su localhost.</span><span class="sxs-lookup"><span data-stu-id="f35e5-391">The web server starts the app and makes it available on localhost.</span></span>

1. <span data-ttu-id="f35e5-392">Aprire un browser all'indirizzo `http://localhost:<port_number>`.</span><span class="sxs-lookup"><span data-stu-id="f35e5-392">Open a browser to `http://localhost:<port_number>`.</span></span> <span data-ttu-id="f35e5-393">Il file *wwwroot/index.html* viene servito.</span><span class="sxs-lookup"><span data-stu-id="f35e5-393">The *wwwroot/index.html* file is served.</span></span> <span data-ttu-id="f35e5-394">Copiare l'URL dalla barra dell'indirizzo.</span><span class="sxs-lookup"><span data-stu-id="f35e5-394">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="f35e5-395">Aprire un'altra istanza del browser (qualsiasi browser).</span><span class="sxs-lookup"><span data-stu-id="f35e5-395">Open another browser instance (any browser).</span></span> <span data-ttu-id="f35e5-396">Incollare l'URL nella barra degli indirizzi.</span><span class="sxs-lookup"><span data-stu-id="f35e5-396">Paste the URL in the address bar.</span></span>

1. <span data-ttu-id="f35e5-397">Scegliere un browser, digitare qualcosa nella casella di testo **Messaggio** e fare clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="f35e5-397">Choose either browser, type something in the **Message** text box, and click the **Send** button.</span></span> <span data-ttu-id="f35e5-398">Il nome utente e il messaggio univoci vengono visualizzati immediatamente in entrambe le pagine.</span><span class="sxs-lookup"><span data-stu-id="f35e5-398">The unique user name and message are displayed on both pages instantly.</span></span>

---

![Messaggio visualizzato in entrambe le finestre del browser](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="f35e5-400">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f35e5-400">Additional resources</span></span>

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
