---
title: Compilazione di progetti con Yeoman in ASP.NET Core
author: spboyer
description: In questo articolo illustra come generare un ASP.NET Core applicazione web tramite il Yeoman generatore in macOS.
keywords: ASP.NET Core, Yeoman, multipiattaforma, messaggio aspnet
ms.author: spboyer
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: fda0c2a8-1743-4505-be1a-7f8ceeef8647
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/yeoman
ms.openlocfilehash: 3a7cd83becc570d2f73014b356977fedb16f29bb
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-building-projects-with-yeoman-in-aspnet-core"></a><span data-ttu-id="5b547-104">Introduzione alla creazione di progetti con Yeoman in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5b547-104">Introduction to building projects with Yeoman in ASP.NET Core</span></span>

<span data-ttu-id="5b547-105">[Yeoman](http://yeoman.io/) è un sistema di scaffolding di progetto per la creazione di diversi tipi di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="5b547-105">[Yeoman](http://yeoman.io/) is a project scaffolding system for creating many kinds of applications.</span></span> <span data-ttu-id="5b547-106">Il Yeoman generatore per ASP.NET Core contiene una varietà di modelli di progetto per l'avvio di un nuovo sito web, MVC o l'applicazione console.</span><span class="sxs-lookup"><span data-stu-id="5b547-106">The Yeoman generator for ASP.NET Core contains a variety of project templates for starting a new web, MVC, or console application.</span></span>

## <a name="install-nodejs-npm-and-yeoman"></a><span data-ttu-id="5b547-107">Installazione di Node.js e npm Yeoman</span><span class="sxs-lookup"><span data-stu-id="5b547-107">Install Node.js, npm, and Yeoman</span></span>

### <a name="prerequisites"></a><span data-ttu-id="5b547-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="5b547-108">Prerequisites</span></span>

<span data-ttu-id="5b547-109">Sono necessari per Yeoman Node.js e npm.</span><span class="sxs-lookup"><span data-stu-id="5b547-109">Node.js and npm are required for Yeoman.</span></span> <span data-ttu-id="5b547-110">Scaricare da [Node.js](https://nodejs.org/en/).</span><span class="sxs-lookup"><span data-stu-id="5b547-110">Download from [Node.js](https://nodejs.org/en/).</span></span> <span data-ttu-id="5b547-111">Il programma di installazione include [Node.js](https://nodejs.org/en/) e [npm](https://www.npmjs.com/).</span><span class="sxs-lookup"><span data-stu-id="5b547-111">The installer includes [Node.js](https://nodejs.org/en/) and [npm](https://www.npmjs.com/).</span></span> <span data-ttu-id="5b547-112">È necessario per l'installazione di Framework dell'interfaccia utente come Bootstrap anche bower.</span><span class="sxs-lookup"><span data-stu-id="5b547-112">Bower is also required for installing UI frameworks like Bootstrap.</span></span>

<span data-ttu-id="5b547-113">Per installare Yeoman e Bower, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="5b547-113">To install Yeoman and Bower, run the following command:</span></span>

```console
npm install -g yo bower
```

>[!Note]
><span data-ttu-id="5b547-114">Se viene visualizzato l'errore `npm ERR! Please try running this command again as root/Administrator.` in macOS, eseguire il comando seguente utilizzando [sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html):`sudo npm install -g yo bower`</span><span class="sxs-lookup"><span data-stu-id="5b547-114">If you get the error `npm ERR! Please try running this command again as root/Administrator.` on macOS, run the following command using [sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html): `sudo npm install -g yo bower`</span></span>

<span data-ttu-id="5b547-115">Da un prompt dei comandi, installare il generatore ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="5b547-115">From a command prompt, install the ASP.NET generator:</span></span>

```console
npm install -g generator-aspnet
```

> [!NOTE]
> <span data-ttu-id="5b547-116">Se si verifica un errore di autorizzazione, eseguire il comando in `sudo` come descritto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="5b547-116">If you get a permission error, run the command under `sudo` as described above.</span></span>

<span data-ttu-id="5b547-117">Il `–g` flag installa il generatore a livello globale, in modo che può essere utilizzato da qualsiasi percorso.</span><span class="sxs-lookup"><span data-stu-id="5b547-117">The `–g` flag installs the generator globally, so that it can be used from any path.</span></span>

## <a name="create-an-aspnet-app"></a><span data-ttu-id="5b547-118">Creare un'applicazione ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5b547-118">Create an ASP.NET app</span></span>

<span data-ttu-id="5b547-119">Eseguire il generatore di ASP.NET basato su Yeoman:</span><span class="sxs-lookup"><span data-stu-id="5b547-119">Run the Yeoman-based ASP.NET generator:</span></span>

```console
yo aspnet
```

<span data-ttu-id="5b547-120">Il generatore di Visualizza un menu.</span><span class="sxs-lookup"><span data-stu-id="5b547-120">The generator displays a menu.</span></span> <span data-ttu-id="5b547-121">Freccia verso il basso per il **Web Application base [senza l'appartenenza e l'autorizzazione]** del progetto e toccare **invio**:</span><span class="sxs-lookup"><span data-stu-id="5b547-121">Arrow down to the **Web Application Basic [without Membership and Authorization]** project and tap **Enter**:</span></span>

![Finestra di comando: il tipo di applicazione si desidera creare?](yeoman/_static/yeoman-yo-aspnet.png)

<span data-ttu-id="5b547-124">Selezionare Bootstrap del framework dell'interfaccia utente e toccare **invio**.</span><span class="sxs-lookup"><span data-stu-id="5b547-124">Select Bootstrap as the UI Framework and tap **Enter**.</span></span>

<span data-ttu-id="5b547-125">Utilizzare "**MyWebApp**" per il nome dell'applicazione e quindi toccare **invio**.</span><span class="sxs-lookup"><span data-stu-id="5b547-125">Use "**MyWebApp**" for the app name and then tap **Enter**.</span></span>

<span data-ttu-id="5b547-126">Yeoman verrà lo scaffolding al progetto e i relativi file di supporto.</span><span class="sxs-lookup"><span data-stu-id="5b547-126">Yeoman will scaffold the project and its supporting files.</span></span> <span data-ttu-id="5b547-127">Passaggi successivi suggeriti vengono inoltre forniti sotto forma di comandi.</span><span class="sxs-lookup"><span data-stu-id="5b547-127">Suggested next steps are also provided in the form of commands.</span></span>

![Finestra di comando: che cos'è il nome dell'applicazione ASP.NET?](yeoman/_static/yeoman-yo-aspnet-created.png)

<span data-ttu-id="5b547-130">Il [generatore ASP.NET](https://www.npmjs.com/package/generator-aspnet) crea ASP.NET Core progetti che possono essere caricato nel codice di Visual Studio, Visual Studio, o eseguire dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="5b547-130">The [ASP.NET generator](https://www.npmjs.com/package/generator-aspnet) creates ASP.NET Core projects that can be loaded into Visual Studio Code, Visual Studio, or run from the command line.</span></span>

## <a name="restore-build-and-run"></a><span data-ttu-id="5b547-131">Il ripristino, compilare ed eseguire</span><span class="sxs-lookup"><span data-stu-id="5b547-131">Restore, build, and run</span></span>

<span data-ttu-id="5b547-132">Seguire i comandi suggeriti modificando la directory in cui il `MyWebApp` directory.</span><span class="sxs-lookup"><span data-stu-id="5b547-132">Follow the suggested commands by changing directories to the `MyWebApp` directory.</span></span> <span data-ttu-id="5b547-133">Eseguire quindi `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="5b547-133">Then run `dotnet restore`.</span></span>

![finestra di comando](yeoman/_static/dotnet-restore.png)

<span data-ttu-id="5b547-135">Compilare ed eseguire l'app mediante `dotnet build` e `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="5b547-135">Build and run the app using `dotnet build` and `dotnet run`:</span></span>

![finestra di comando](yeoman/_static/dotnet-build-run.png)

<span data-ttu-id="5b547-137">A questo punto, è possibile passare all'URL indicato per testare l'app di ASP.NET Core appena creato.</span><span class="sxs-lookup"><span data-stu-id="5b547-137">At this point, you can navigate to the URL shown to test the newly-created ASP.NET Core app.</span></span>

## <a name="client-side-packages"></a><span data-ttu-id="5b547-138">Pacchetti sul lato client</span><span class="sxs-lookup"><span data-stu-id="5b547-138">Client-side packages</span></span>

<span data-ttu-id="5b547-139">Le risorse front-end vengono fornite tramite i modelli dal Yeoman utilizzando Generatore di [Bower](xref:client-side/bower) gestione di pacchetti sul lato client, aggiungere *bower. JSON* e *. bowerrc* file da ripristinare i pacchetti sul lato client tramite Bower.</span><span class="sxs-lookup"><span data-stu-id="5b547-139">The front-end resources are provided by the templates from the Yeoman generator using the [Bower](xref:client-side/bower) client-side package manager, adding *bower.json* and *.bowerrc* files to restore client-side packages using Bower.</span></span>

<span data-ttu-id="5b547-140">Il [BundlerMinifier](xref:client-side/bundling-and-minification) componente è inoltre incluso per impostazione predefinita per facilità di concatenazione (aggregazione) e la minimizzazione CSS, JavaScript e HTML.</span><span class="sxs-lookup"><span data-stu-id="5b547-140">The [BundlerMinifier](xref:client-side/bundling-and-minification) component is also included by default for ease of concatenation (bundling) and minification of CSS, JavaScript, and HTML.</span></span>

## <a name="building-and-running-from-visual-studio"></a><span data-ttu-id="5b547-141">Compilazione e l'esecuzione da Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b547-141">Building and running from Visual Studio</span></span>

<span data-ttu-id="5b547-142">È possibile caricare il progetto web ASP.NET Core generato direttamente in Visual Studio, quindi compilare ed eseguire il progetto da qui.</span><span class="sxs-lookup"><span data-stu-id="5b547-142">You can load your generated ASP.NET Core web project directly into Visual Studio, then build and run your project from there.</span></span> <span data-ttu-id="5b547-143">Seguire le istruzioni sopra riportate per eseguire lo scaffolding di una nuova app di ASP.NET Core utilizzando Yeoman.</span><span class="sxs-lookup"><span data-stu-id="5b547-143">Follow the instructions above to scaffold a new ASP.NET Core app using Yeoman.</span></span> <span data-ttu-id="5b547-144">Questa volta scegliere **applicazione Web** nel menu e il nome dell'app `MyWebApp`.</span><span class="sxs-lookup"><span data-stu-id="5b547-144">This time, choose **Web Application** from the menu and name the app `MyWebApp`.</span></span>

<span data-ttu-id="5b547-145">Aprire Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5b547-145">Open Visual Studio.</span></span> <span data-ttu-id="5b547-146">Dal menu File, selezionare ‣ Apri progetto/soluzione.</span><span class="sxs-lookup"><span data-stu-id="5b547-146">From the File menu, select Open ‣ Project/Solution.</span></span>

<span data-ttu-id="5b547-147">Nella finestra di dialogo Apri progetto individuare il *csproj* file, selezionarlo e fare clic su di **aprire** pulsante.</span><span class="sxs-lookup"><span data-stu-id="5b547-147">In the Open Project dialog, navigate to the *.csproj* file, select it, and click the **Open** button.</span></span> <span data-ttu-id="5b547-148">In Esplora soluzioni, il progetto dovrebbe essere simile nella schermata seguente.</span><span class="sxs-lookup"><span data-stu-id="5b547-148">In the Solution Explorer, the project should look something like the screenshot below.</span></span>

![File e cartelle di un nuovo progetto in Esplora soluzioni](yeoman/_static/yeoman-solution.png)

<span data-ttu-id="5b547-150">Yeoman scaffolds un'applicazione web MVC, completo sia lato client e server di supporto per la compilazione.</span><span class="sxs-lookup"><span data-stu-id="5b547-150">Yeoman scaffolds a MVC web application, complete with both server- and client-side build support.</span></span> <span data-ttu-id="5b547-151">Dipendenze sul lato server sono elencate sotto la **dipendenze/NuGet** nodo e le dipendenze sul lato client il **dipendenze/Bower** nodo di Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="5b547-151">Server-side dependencies are listed under the **Dependencies/NuGet** node, and client-side dependencies in the **Dependencies/Bower** node of Solution Explorer.</span></span> <span data-ttu-id="5b547-152">Le dipendenze vengono ripristinate automaticamente quando viene caricato il progetto.</span><span class="sxs-lookup"><span data-stu-id="5b547-152">Dependencies are restored automatically when the project is loaded.</span></span>

![Sotto il nodo di dipendenze in visualizzazione struttura ad albero di Esplora soluzioni, la cartella Bower è aprire l'elenco delle relative dipendenze.](yeoman/_static/yeoman-loading-dependencies.png)

<span data-ttu-id="5b547-154">Quando tutte le dipendenze vengono ripristinate, premere **F5** per eseguire il progetto.</span><span class="sxs-lookup"><span data-stu-id="5b547-154">When all the dependencies are restored, press **F5** to run the project.</span></span> <span data-ttu-id="5b547-155">Consente di visualizzare la home page predefinita nel browser.</span><span class="sxs-lookup"><span data-stu-id="5b547-155">The default home page displays in the browser.</span></span>

![Applicazione Web aperto in Microsoft Edge](yeoman/_static/yeoman-home-page.png)

## <a name="restoring-building-and-hosting-from-a-command-line"></a><span data-ttu-id="5b547-157">Il ripristino, la creazione e l'hosting dalla riga di comando</span><span class="sxs-lookup"><span data-stu-id="5b547-157">Restoring, building, and hosting from a command line</span></span>

<span data-ttu-id="5b547-158">È possibile preparare e ospitare l'applicazione web tramite l'interfaccia CLI di .NET Core.</span><span class="sxs-lookup"><span data-stu-id="5b547-158">You can prepare and host your web application using the .NET Core CLI.</span></span>

<span data-ttu-id="5b547-159">Al prompt dei comandi, modificare la directory corrente alla cartella contenente il progetto (vale a dire la cartella contenente il *csproj* file):</span><span class="sxs-lookup"><span data-stu-id="5b547-159">At a command prompt, change the current directory to the folder containing the project (that is, the folder containing the *.csproj* file):</span></span>

```console
cd src\MyWebApp
```

<span data-ttu-id="5b547-160">Ripristinare le dipendenze di pacchetto NuGet del progetto:</span><span class="sxs-lookup"><span data-stu-id="5b547-160">Restore the project's NuGet package dependencies:</span></span>

```console
dotnet restore
```

<span data-ttu-id="5b547-161">Eseguire l'applicazione:</span><span class="sxs-lookup"><span data-stu-id="5b547-161">Run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="5b547-162">La piattaforma incrociata [Kestrel](xref:fundamentals/servers/kestrel) server web verrà mettersi in ascolto sulla porta 5000.</span><span class="sxs-lookup"><span data-stu-id="5b547-162">The cross-platform [Kestrel](xref:fundamentals/servers/kestrel) web server will begin listening on port 5000.</span></span>

<span data-ttu-id="5b547-163">Aprire un web browser e passare a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="5b547-163">Open a web browser, and navigate to `http://localhost:5000`.</span></span>

![Applicazione Web aperto in Microsoft Edge](yeoman/_static/yeoman-home-page_5000.png)

## <a name="adding-to-your-project-with-sub-generators"></a><span data-ttu-id="5b547-165">Aggiunta a un progetto con i generatori di sub</span><span class="sxs-lookup"><span data-stu-id="5b547-165">Adding to your project with sub generators</span></span>

<span data-ttu-id="5b547-166">Utilizzando Yeoman [sub generatori](https://www.github.com/omnisharp/generator-aspnet#sub-generators), è possibile aggiungere un `nuget.config` o `web.config` dopo la creazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="5b547-166">Using Yeoman [sub generators](https://www.github.com/omnisharp/generator-aspnet#sub-generators), you can add either a `nuget.config` or a `web.config` after the project is created.</span></span> <span data-ttu-id="5b547-167">Ad esempio, eseguire il comando seguente dalla directory in cui deve essere creato il file:</span><span class="sxs-lookup"><span data-stu-id="5b547-167">For example, execute the following command from the directory in which the file should be created:</span></span>

```console
yo aspnet:nugetconfig
```

<span data-ttu-id="5b547-168">Il risultato è un file di configurazione NuGet denominato `nuget.config` con il seguente contenuto:</span><span class="sxs-lookup"><span data-stu-id="5b547-168">The result is a NuGet configuration file named `nuget.config` with the following content:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
 <packageSources>
    <!--To inherit the global NuGet package sources remove the <clear/> line below -->
    <clear />
 </packageSources>
</configuration>
```

## <a name="additional-resources"></a><span data-ttu-id="5b547-169">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="5b547-169">Additional resources</span></span>

* [<span data-ttu-id="5b547-170">Server (Kestrel WebListener)</span><span class="sxs-lookup"><span data-stu-id="5b547-170">Servers (Kestrel and WebListener)</span></span>](xref:fundamentals/servers/index)
* [<span data-ttu-id="5b547-171">Nozioni di base</span><span class="sxs-lookup"><span data-stu-id="5b547-171">Fundamentals</span></span>](xref:fundamentals/index)
