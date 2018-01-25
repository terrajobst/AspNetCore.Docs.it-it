---
title: Utilizzare il modello di progetto React
author: SteveSandersonMS
description: Informazioni su come iniziare con il modello di progetto ASP.NET Core a pagina singola applicazione (SPA) versione finale candidata per React e creare app di react.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/06/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react
ms.openlocfilehash: 5978094083a098a771f5dca103434ea8fcce7777
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="use-the-react-project-template-release-candidate"></a><span data-ttu-id="01dfa-103">Utilizzare il modello di progetto React (versione finale candidata)</span><span class="sxs-lookup"><span data-stu-id="01dfa-103">Use the React project template (release candidate)</span></span>

> [!NOTE]
> <span data-ttu-id="01dfa-104">Questa documentazione non è sul modello di progetto React rilasciato.</span><span class="sxs-lookup"><span data-stu-id="01dfa-104">This documentation isn't about the released React project template.</span></span> <span data-ttu-id="01dfa-105">**Questa documentazione è sulla versione finale candidata del modello React.**</span><span class="sxs-lookup"><span data-stu-id="01dfa-105">**This documentation is about the release candidate of the React template.**</span></span> <span data-ttu-id="01dfa-106">Ci auguriamo che per la versione rilasciata 2018 anticipata.</span><span class="sxs-lookup"><span data-stu-id="01dfa-106">We hope to ship the released version in early 2018.</span></span>

<span data-ttu-id="01dfa-107">Il modello di progetto aggiornato React fornisce un punto di partenza ideale per ASP.NET Core App scritte in React e [creare app di reazione](https://github.com/facebookincubator/create-react-app) convenzioni (CRA) per implementare un'interfaccia utente avanzata, sul lato client (UI).</span><span class="sxs-lookup"><span data-stu-id="01dfa-107">The updated React project template provides a convenient starting point for ASP.NET Core apps using React and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="01dfa-108">Il modello è equivalente alla creazione di un progetto ASP.NET Core per agire come un back-end dell'API e un progetto standard CRA rispondere ad agire come un'interfaccia utente, ma con la praticità di hosting sia in un progetto di app singola che può essere creato e pubblicato come una singola unità.</span><span class="sxs-lookup"><span data-stu-id="01dfa-108">The template is equivalent to creating both an ASP.NET Core project to act as an API backend, and a standard CRA React project to act as a UI, but with the convenience of hosting both in a single app project that can be built and published as a single unit.</span></span>

## <a name="create-a-new-app"></a><span data-ttu-id="01dfa-109">Creare una nuova app</span><span class="sxs-lookup"><span data-stu-id="01dfa-109">Create a new app</span></span>

<span data-ttu-id="01dfa-110">Per iniziare, assicurarsi di aver [installato il modello di progetto aggiornato React](xref:spa/index#installation).</span><span class="sxs-lookup"><span data-stu-id="01dfa-110">To get started, ensure you've [installed the updated React project template](xref:spa/index#installation).</span></span> <span data-ttu-id="01dfa-111">Queste istruzioni non si applicano al modello di progetto React precedente incluso in .NET Core SDK 2.0.</span><span class="sxs-lookup"><span data-stu-id="01dfa-111">These instructions don't apply to the previous React project template included in the .NET Core 2.0.x SDK.</span></span>

<span data-ttu-id="01dfa-112">Creare un nuovo progetto da un prompt dei comandi utilizzando il comando `dotnet new react` in una directory vuota.</span><span class="sxs-lookup"><span data-stu-id="01dfa-112">Create a new project from a command prompt using the command `dotnet new react` in an empty directory.</span></span> <span data-ttu-id="01dfa-113">Ad esempio, i comandi seguenti creano l'app in un *my-nuova-app* directory e passare alla directory:</span><span class="sxs-lookup"><span data-stu-id="01dfa-113">For example, the following commands create the app in a *my-new-app* directory and switch to that directory:</span></span>

```console
dotnet new react -o my-new-app
cd my-new-app
```

<span data-ttu-id="01dfa-114">Eseguire l'app da Visual Studio o l'interfaccia CLI di .NET Core:</span><span class="sxs-lookup"><span data-stu-id="01dfa-114">Run the app from either Visual Studio or the .NET Core CLI:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="01dfa-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01dfa-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="01dfa-116">Aprire generato *csproj* file ed eseguire l'app come al solito da qui.</span><span class="sxs-lookup"><span data-stu-id="01dfa-116">Open the generated *.csproj* file, and run the app as normal from there.</span></span>

<span data-ttu-id="01dfa-117">Il processo di compilazione consente di ripristinare le dipendenze di npm alla prima esecuzione, che può richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="01dfa-117">The build process restores npm dependencies on the first run, which can take several minutes.</span></span> <span data-ttu-id="01dfa-118">Le compilazioni successive sono molto più veloce.</span><span class="sxs-lookup"><span data-stu-id="01dfa-118">Subsequent builds are much faster.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="01dfa-119">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="01dfa-119">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="01dfa-120">Assicurarsi di disporre di una variabile di ambiente denominata `ASPNETCORE_Environment` con valore `Development`.</span><span class="sxs-lookup"><span data-stu-id="01dfa-120">Ensure you have an environment variable called `ASPNETCORE_Environment` with value of `Development`.</span></span> <span data-ttu-id="01dfa-121">In Windows (in istruzioni non PowerShell), eseguire `SET ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="01dfa-121">On Windows (in non-PowerShell prompts), run `SET ASPNETCORE_Environment=Development`.</span></span> <span data-ttu-id="01dfa-122">In Linux o Mac OS, eseguire `export ASPNETCORE_Environment=Development`.</span><span class="sxs-lookup"><span data-stu-id="01dfa-122">On Linux or macOS, run `export ASPNETCORE_Environment=Development`.</span></span>

<span data-ttu-id="01dfa-123">Eseguire `dotnet build` per verificare l'applicazione venga compilata correttamente.</span><span class="sxs-lookup"><span data-stu-id="01dfa-123">Run `dotnet build` to verify your app builds correctly.</span></span> <span data-ttu-id="01dfa-124">Alla prima esecuzione, il processo di compilazione consente di ripristinare le dipendenze di npm, che possono richiedere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="01dfa-124">On the first run, the build process restores npm dependencies, which can take several minutes.</span></span> <span data-ttu-id="01dfa-125">Le compilazioni successive sono molto più veloce.</span><span class="sxs-lookup"><span data-stu-id="01dfa-125">Subsequent builds are much faster.</span></span>

<span data-ttu-id="01dfa-126">Eseguire `dotnet run` per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="01dfa-126">Run `dotnet run` to start the app.</span></span>

---

<span data-ttu-id="01dfa-127">Il modello di progetto crea un'applicazione ASP.NET Core e un'app React.</span><span class="sxs-lookup"><span data-stu-id="01dfa-127">The project template creates an ASP.NET Core app and a React app.</span></span> <span data-ttu-id="01dfa-128">L'applicazione ASP.NET di base deve essere utilizzato per l'accesso ai dati, l'autorizzazione e altri problemi sul lato server.</span><span class="sxs-lookup"><span data-stu-id="01dfa-128">The ASP.NET Core app is intended to be used for data access, authorization, and other server-side concerns.</span></span> <span data-ttu-id="01dfa-129">L'app React, che si trovano nel *ClientApp* sottodirectory, dovrà essere utilizzato per tutti i problemi dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="01dfa-129">The React app, residing in the *ClientApp* subdirectory, is intended to be used for all UI concerns.</span></span>

## <a name="add-pages-images-styles-modules-etc"></a><span data-ttu-id="01dfa-130">Aggiungere pagine, immagini, stili, moduli e così via.</span><span class="sxs-lookup"><span data-stu-id="01dfa-130">Add pages, images, styles, modules, etc.</span></span>

<span data-ttu-id="01dfa-131">Il *ClientApp* directory è un'applicazione di rispondere CRA standard.</span><span class="sxs-lookup"><span data-stu-id="01dfa-131">The *ClientApp* directory is a standard CRA React app.</span></span> <span data-ttu-id="01dfa-132">Vedere ufficiali [documentazione CRA](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="01dfa-132">See the official [CRA documentation](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md) for more information.</span></span>

<span data-ttu-id="01dfa-133">Vi sono piccole differenze tra l'app React creato da questo modello e quello creato da CRA stesso. Tuttavia, non vengono modificate le funzionalità dell'app.</span><span class="sxs-lookup"><span data-stu-id="01dfa-133">There are slight differences between the React app created by this template and the one created by CRA itself; however, the app's capabilities are unchanged.</span></span> <span data-ttu-id="01dfa-134">L'app creata tramite il modello contiene un [Bootstrap](https://getbootstrap.com/)-base di layout e un esempio di routing di base.</span><span class="sxs-lookup"><span data-stu-id="01dfa-134">The app created by the template contains a [Bootstrap](https://getbootstrap.com/)-based layout and a basic routing example.</span></span>

## <a name="install-npm-packages"></a><span data-ttu-id="01dfa-135">Installare pacchetti npm</span><span class="sxs-lookup"><span data-stu-id="01dfa-135">Install npm packages</span></span>

<span data-ttu-id="01dfa-136">Per installare i pacchetti di terze parti npm, utilizzare un prompt dei comandi nel *ClientApp* sottodirectory.</span><span class="sxs-lookup"><span data-stu-id="01dfa-136">To install third-party npm packages, use a command prompt in the *ClientApp* subdirectory.</span></span> <span data-ttu-id="01dfa-137">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="01dfa-137">For example:</span></span>

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a><span data-ttu-id="01dfa-138">Pubblicare e distribuire</span><span class="sxs-lookup"><span data-stu-id="01dfa-138">Publish and deploy</span></span>

<span data-ttu-id="01dfa-139">In fase di sviluppo, l'app viene eseguita in modalità ottimizzata per praticità per sviluppatori.</span><span class="sxs-lookup"><span data-stu-id="01dfa-139">In development, the app runs in a mode optimized for developer convenience.</span></span> <span data-ttu-id="01dfa-140">Ad esempio, JavaScript bundle includono mapping di origine (in modo che durante il debug, è possibile visualizzare il codice sorgente originale).</span><span class="sxs-lookup"><span data-stu-id="01dfa-140">For example, JavaScript bundles include source maps (so that when debugging, you can see your original source code).</span></span> <span data-ttu-id="01dfa-141">L'applicazione verifica la presenza di JavaScript, HTML e CSS file modifiche disco automaticamente ricompilazioni e ricarica quando rileva tali file modificare.</span><span class="sxs-lookup"><span data-stu-id="01dfa-141">The app watches JavaScript, HTML, and CSS file changes on disk and automatically recompiles and reloads when it sees those files change.</span></span>

<span data-ttu-id="01dfa-142">Nell'ambiente di produzione, utilizzare una versione dell'app che è ottimizzato per le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="01dfa-142">In production, serve a version of your app that's optimized for performance.</span></span> <span data-ttu-id="01dfa-143">Questo è configurato per eseguire automaticamente.</span><span class="sxs-lookup"><span data-stu-id="01dfa-143">This is configured to happen automatically.</span></span> <span data-ttu-id="01dfa-144">Quando si pubblica, la configurazione della build genera una compilazione transpiled minimizzata, del codice sul lato client.</span><span class="sxs-lookup"><span data-stu-id="01dfa-144">When you publish, the build configuration emits a minified, transpiled build of your client-side code.</span></span> <span data-ttu-id="01dfa-145">A differenza della compilazione di sviluppo, la compilazione di produzione non richiede Node.js essere installato nel server.</span><span class="sxs-lookup"><span data-stu-id="01dfa-145">Unlike the development build, the production build doesn't require Node.js to be installed on the server.</span></span>

<span data-ttu-id="01dfa-146">È possibile utilizzare standard [metodi di distribuzione e hosting ASP.NET Core](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="01dfa-146">You can use standard [ASP.NET Core hosting and deployment methods](xref:host-and-deploy/index).</span></span>

## <a name="run-the-cra-server-independently"></a><span data-ttu-id="01dfa-147">Eseguire in modo indipendente il server CRA</span><span class="sxs-lookup"><span data-stu-id="01dfa-147">Run the CRA server independently</span></span>

<span data-ttu-id="01dfa-148">Il progetto è configurato per avviare la propria istanza del server di sviluppo CRA in background all'avvio dell'app di ASP.NET Core in modalità di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="01dfa-148">The project is configured to start its own instance of the CRA development server in the background when the ASP.NET Core app starts in development mode.</span></span> <span data-ttu-id="01dfa-149">Ciò risulta utile in quanto significa che non è necessario eseguire manualmente un server separato.</span><span class="sxs-lookup"><span data-stu-id="01dfa-149">This is convenient because it means you don't have to run a separate server manually.</span></span>

<span data-ttu-id="01dfa-150">È uno svantaggio di questo programma di installazione predefinito.</span><span class="sxs-lookup"><span data-stu-id="01dfa-150">There's a drawback to this default setup.</span></span> <span data-ttu-id="01dfa-151">Ogni volta che si modifica il codice c# e il ASP.NET Core app richiede il riavvio, il server CRA viene riavviato.</span><span class="sxs-lookup"><span data-stu-id="01dfa-151">Each time you modify your C# code and your ASP.NET Core app needs to restart, the CRA server restarts.</span></span> <span data-ttu-id="01dfa-152">Per avviare il backup, sono necessari alcuni secondi.</span><span class="sxs-lookup"><span data-stu-id="01dfa-152">A few seconds are required to start back up.</span></span> <span data-ttu-id="01dfa-153">Se si desidera apportare frequenti modifiche al codice c# e non si desidera attendere il riavvio del server CRA, eseguire il server CRA esternamente, indipendentemente dal processo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="01dfa-153">If you're making frequent C# code edits and don't want to wait for the CRA server to restart, run the CRA server externally, independently of the ASP.NET Core process.</span></span> <span data-ttu-id="01dfa-154">A tale scopo:</span><span class="sxs-lookup"><span data-stu-id="01dfa-154">To do so:</span></span>

1. <span data-ttu-id="01dfa-155">In un prompt dei comandi, passare il *ClientApp* sottodirectory e avviare il server di sviluppo CRA:</span><span class="sxs-lookup"><span data-stu-id="01dfa-155">In a command prompt, switch to the *ClientApp* subdirectory, and launch the CRA development server:</span></span>

    ```console
    cd ClientApp
    npm start
    ```

2. <span data-ttu-id="01dfa-156">Modificare l'applicazione ASP.NET Core per utilizzare l'istanza del server esterno CRA anziché avviare uno dei propri.</span><span class="sxs-lookup"><span data-stu-id="01dfa-156">Modify your ASP.NET Core app to use the external CRA server instance instead of launching one of its own.</span></span> <span data-ttu-id="01dfa-157">Nel *avvio* classe, sostituire il `spa.UseReactDevelopmentServer` chiamata con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="01dfa-157">In your *Startup* class, replace the `spa.UseReactDevelopmentServer` invocation with the following:</span></span>

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

<span data-ttu-id="01dfa-158">Quando si avvia l'app ASP.NET Core, non verrà avviato un server CRA.</span><span class="sxs-lookup"><span data-stu-id="01dfa-158">When you start your ASP.NET Core app, it won't launch a CRA server.</span></span> <span data-ttu-id="01dfa-159">Viene utilizzato l'istanza che è stato avviato manualmente.</span><span class="sxs-lookup"><span data-stu-id="01dfa-159">The instance you started manually is used instead.</span></span> <span data-ttu-id="01dfa-160">Ciò consente di avviare e riavviare più velocemente.</span><span class="sxs-lookup"><span data-stu-id="01dfa-160">This enables it to start and restart faster.</span></span> <span data-ttu-id="01dfa-161">Non è in attesa per l'app React ricompilare ogni volta.</span><span class="sxs-lookup"><span data-stu-id="01dfa-161">It's no longer waiting for your React app to rebuild each time.</span></span>
