---
uid: mvc/overview/deployment/docker
title: Migrazione di applicazioni ASP.NET MVC ai contenitori di Windows
description: Informazioni su come eseguire un'applicazione ASP.NET MVC esistente in un contenitore Docker di Windows
keywords: Windows Containers,Docker,ASP.NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 02/01/2017
ms.topic: article
ms.prod: .net-framework
ms.technology: dotnet-mvc
ms.devlang: dotnet
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: 336d0e9d2247dd2d63c779fd446f9a50be6dbc50
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a><span data-ttu-id="55344-104">Migrazione di applicazioni ASP.NET MVC ai contenitori di Windows</span><span class="sxs-lookup"><span data-stu-id="55344-104">Migrating ASP.NET MVC Applications to Windows Containers</span></span>

<span data-ttu-id="55344-105">Per l'esecuzione di un'applicazione basata su .NET Framework esistente in un contenitore di Windows non sono richieste modifiche dell'app.</span><span class="sxs-lookup"><span data-stu-id="55344-105">Running an existing .NET Framework-based application in a Windows container doesn't require any changes to your app.</span></span> <span data-ttu-id="55344-106">Per eseguire l'app in un contenitore di Windows si crea un'immagine Docker contenente l'app e si avvia il contenitore.</span><span class="sxs-lookup"><span data-stu-id="55344-106">To run your app in a Windows container you create a Docker image containing your app and start the container.</span></span> <span data-ttu-id="55344-107">Questo argomento illustra come distribuire un'[applicazione ASP.NET MVC](http://www.asp.net/mvc) esistente in un contenitore di Windows.</span><span class="sxs-lookup"><span data-stu-id="55344-107">This topic explains how to take an existing [ASP.NET MVC application](http://www.asp.net/mvc) and deploy it in a Windows container.</span></span>

<span data-ttu-id="55344-108">Si parte da un'app esistente ASP.NET MVC e quindi si compilano gli asset pubblicati usando Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="55344-108">You start with an existing ASP.NET MVC app, then build the published assets using Visual Studio.</span></span> <span data-ttu-id="55344-109">Si usa Docker per creare l'immagine che contiene ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="55344-109">You use Docker to create the image that contains and runs your app.</span></span> <span data-ttu-id="55344-110">Si passerà poi al sito in esecuzione in un contenitore di Windows per verificare il funzionamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="55344-110">You'll browse to the site running in a Windows container and verify the app is working.</span></span>

<span data-ttu-id="55344-111">In questo articolo si presuppone che l'utente abbia una conoscenza di base di Docker.</span><span class="sxs-lookup"><span data-stu-id="55344-111">This article assumes a basic understanding of Docker.</span></span> <span data-ttu-id="55344-112">Per informazioni su Docker, è possibile leggere la [panoramica su Docker](https://docs.docker.com/engine/understanding-docker/).</span><span class="sxs-lookup"><span data-stu-id="55344-112">You can learn about Docker by reading the [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span></span>

<span data-ttu-id="55344-113">L'app che verrà eseguita in un contenitore è un semplice sito Web che risponde alle domande in modo casuale.</span><span class="sxs-lookup"><span data-stu-id="55344-113">The app you'll run in a container is a simple website that answers questions randomly.</span></span> <span data-ttu-id="55344-114">Questa app è un'applicazione MVC di base senza supporto per l'autenticazione né l'archiviazione in database, che consente di concentrarsi sullo spostamento del livello Web in un contenitore.</span><span class="sxs-lookup"><span data-stu-id="55344-114">This app is a basic MVC application with no authentication or database storage; it lets you focus on moving the web tier to a container.</span></span> <span data-ttu-id="55344-115">Gli argomenti successivi spiegheranno come spostare e gestire l'archiviazione permanente nelle applicazioni eseguite nei contenitori.</span><span class="sxs-lookup"><span data-stu-id="55344-115">Future topics will show how to move and manage persistent storage in containerized applications.</span></span>

<span data-ttu-id="55344-116">Per spostare l'applicazione sono necessari i passaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="55344-116">Moving your application involves these steps:</span></span>

1. [<span data-ttu-id="55344-117">Creazione di un'attività di pubblicazione per compilare le risorse necessarie per un'immagine.</span><span class="sxs-lookup"><span data-stu-id="55344-117">Creating a publish task to build the assets for an image.</span></span>](#publish-script)
1. [<span data-ttu-id="55344-118">Creazione di un'immagine Docker che eseguirà l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="55344-118">Building a Docker image that will run your application.</span></span>](#build-the-image)
1. [<span data-ttu-id="55344-119">Avvio di un contenitore Docker che esegue l'immagine.</span><span class="sxs-lookup"><span data-stu-id="55344-119">Starting a Docker container that runs your image.</span></span>](#start-a-container)
1. [<span data-ttu-id="55344-120">Verifica dell'applicazione con il browser.</span><span class="sxs-lookup"><span data-stu-id="55344-120">Verifying the application using your browser.</span></span>](#verify-in-the-browser)

<span data-ttu-id="55344-121">L'[applicazione completata](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator) è disponibile in GitHub.</span><span class="sxs-lookup"><span data-stu-id="55344-121">The [finished application](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator) is on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="55344-122">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="55344-122">Prerequisites</span></span>

<span data-ttu-id="55344-123">Il computer di sviluppo deve disporre di</span><span class="sxs-lookup"><span data-stu-id="55344-123">The development machine must be running</span></span>

- <span data-ttu-id="55344-124">[Aggiornamento dell'anniversario di Windows 10](https://www.microsoft.com/en-us/software-download/windows10/) (o versione successiva) oppure [Windows Server 2016](https://www.microsoft.com/en-us/cloud-platform/windows-server) (o versione successiva).</span><span class="sxs-lookup"><span data-stu-id="55344-124">[Windows 10 Anniversary Update](https://www.microsoft.com/en-us/software-download/windows10/) (or higher) or [Windows Server 2016](https://www.microsoft.com/en-us/cloud-platform/windows-server) (or higher).</span></span>
- <span data-ttu-id="55344-125">[Docker per Windows](https://docs.docker.com/docker-for-windows/) - versione Stable 1.13.0 o 1.12 Beta 26 (o versioni successive)</span><span class="sxs-lookup"><span data-stu-id="55344-125">[Docker for Windows](https://docs.docker.com/docker-for-windows/) - version Stable 1.13.0 or 1.12 Beta 26 (or newer versions)</span></span>
- <span data-ttu-id="55344-126">[Visual Studio 2017](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="55344-126">[Visual Studio 2017](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="55344-127">Se si usa Windows Server 2016, seguire le istruzioni riportate in [Distribuzione di host contenitore - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).</span><span class="sxs-lookup"><span data-stu-id="55344-127">If you are using Windows Server 2016, follow the instructions for [Container Host Deployment - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).</span></span>

<span data-ttu-id="55344-128">Dopo aver installato e avviato Docker, è necessario fare clic con il pulsante destro del mouse sull'icona della barra delle applicazioni e scegliere l'opzione **Switch to Windows containers** (Passa ai contenitori Windows).</span><span class="sxs-lookup"><span data-stu-id="55344-128">After installing and starting Docker,  right-click on the tray icon and select **Switch to Windows containers**.</span></span> <span data-ttu-id="55344-129">Ciò è necessario per eseguire le immagini Docker basate su Windows.</span><span class="sxs-lookup"><span data-stu-id="55344-129">This is required to run Docker images based on Windows.</span></span> <span data-ttu-id="55344-130">L'esecuzione del comando richiede alcuni secondi:</span><span class="sxs-lookup"><span data-stu-id="55344-130">This command takes a few seconds to execute:</span></span>

<span data-ttu-id="55344-131">![Contenitore di Windows][windows-container]</span><span class="sxs-lookup"><span data-stu-id="55344-131">![Windows Container][windows-container]</span></span>

## <a name="publish-script"></a><span data-ttu-id="55344-132">Pubblicare lo script</span><span class="sxs-lookup"><span data-stu-id="55344-132">Publish script</span></span>

<span data-ttu-id="55344-133">Riunire in un'unica posizione tutti gli asset da caricare in un'immagine Docker.</span><span class="sxs-lookup"><span data-stu-id="55344-133">Collect all the assets that you need to load into a Docker image in one place.</span></span> <span data-ttu-id="55344-134">È possibile usare il comando **Pubblica** di Visual Studio per creare un profilo di pubblicazione per l'app.</span><span class="sxs-lookup"><span data-stu-id="55344-134">You can use the Visual Studio **Publish** command to create a publish profile for your app.</span></span> <span data-ttu-id="55344-135">Tale profilo consente di inserire tutti gli asset in un'unica struttura di directory che verrà copiata nell'immagine di destinazione più avanti in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="55344-135">This profile will put all the assets in one directory tree that you copy to your target image later in this tutorial.</span></span>

<span data-ttu-id="55344-136">**Procedura di pubblicazione**</span><span class="sxs-lookup"><span data-stu-id="55344-136">**Publish Steps**</span></span>

1. <span data-ttu-id="55344-137">Fare clic con il pulsante destro del mouse sul progetto Web in Visual Studio e selezionare **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="55344-137">Right click on the web project in Visual Studio, and select **Publish**.</span></span>
1. <span data-ttu-id="55344-138">Fare clic sul **pulsante del profilo personalizzato** e quindi selezionare **File system** come metodo.</span><span class="sxs-lookup"><span data-stu-id="55344-138">Click the **Custom profile button**, and then select **File System** as the method.</span></span>
1. <span data-ttu-id="55344-139">Scegliere la directory.</span><span class="sxs-lookup"><span data-stu-id="55344-139">Choose the directory.</span></span> <span data-ttu-id="55344-140">Per convenzione, l'esempio scaricato usa `bin\Release\PublishOutput`.</span><span class="sxs-lookup"><span data-stu-id="55344-140">By convention, the downloaded sample uses `bin\Release\PublishOutput`.</span></span>

<span data-ttu-id="55344-141">![Connessione di pubblicazione][publish-connection]</span><span class="sxs-lookup"><span data-stu-id="55344-141">![Publish Connection][publish-connection]</span></span>

<span data-ttu-id="55344-142">Aprire la sezione **Opzioni pubblicazione file** della scheda **Impostazioni**. Selezionare **Precompila durante la pubblicazione**.</span><span class="sxs-lookup"><span data-stu-id="55344-142">Open the **File Publish Options** section of the **Settings** tab. Select **Precompile during publishing**.</span></span> <span data-ttu-id="55344-143">Questa ottimizzazione significa che compilando le viste nel contenitore Docker, si stanno copiando le viste precompilate.</span><span class="sxs-lookup"><span data-stu-id="55344-143">This optimization means that you'll be compiling views in the Docker container, you are copying the precompiled views.</span></span>

<span data-ttu-id="55344-144">![Impostazioni di pubblicazione][publish-settings]</span><span class="sxs-lookup"><span data-stu-id="55344-144">![Publish Settings][publish-settings]</span></span>

<span data-ttu-id="55344-145">Fare clic su **Pubblica** in modo che Visual Studio copi tutti gli asset necessari nella cartella di destinazione.</span><span class="sxs-lookup"><span data-stu-id="55344-145">Click **Publish**, and Visual Studio will copy all the needed assets to the destination folder.</span></span>

## <a name="build-the-image"></a><span data-ttu-id="55344-146">Creare l'immagine</span><span class="sxs-lookup"><span data-stu-id="55344-146">Build the image</span></span>

<span data-ttu-id="55344-147">Definire l'immagine Docker in un Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="55344-147">Define your Docker image in a Dockerfile.</span></span> <span data-ttu-id="55344-148">Il Dockerfile contiene le istruzioni per l'immagine di base, eventuali componenti aggiuntivi, l'app da eseguire e altre immagini di configurazione.</span><span class="sxs-lookup"><span data-stu-id="55344-148">The Dockerfile contains instructions for the base image, additional components, the app you want to run, and other configuration images.</span></span>  <span data-ttu-id="55344-149">Il Dockerfile costituisce l'input per il comando `docker build` che crea l'immagine.</span><span class="sxs-lookup"><span data-stu-id="55344-149">The Dockerfile is the input to the `docker build` command, which creates the image.</span></span>

<span data-ttu-id="55344-150">Verrà creata un'immagine basata sull'immagine `microsft/aspnet` situata nell'[hub Docker](https://hub.docker.com/r/microsoft/aspnet/).</span><span class="sxs-lookup"><span data-stu-id="55344-150">You will build an image based on the `microsft/aspnet` image located on [Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).</span></span>
<span data-ttu-id="55344-151">L'immagine di base, `microsoft/aspnet`, è un'immagine di Windows Server.</span><span class="sxs-lookup"><span data-stu-id="55344-151">The base image, `microsoft/aspnet`, is a Windows Server image.</span></span> <span data-ttu-id="55344-152">Contiene Server Core di Windows, IIS e ASP.NET 4.6.2.</span><span class="sxs-lookup"><span data-stu-id="55344-152">It contains Windows Server Core, IIS and ASP.NET 4.6.2.</span></span> <span data-ttu-id="55344-153">Quando si esegue questa immagine in un contenitore, verranno avviati automaticamente IIS e i siti Web installati.</span><span class="sxs-lookup"><span data-stu-id="55344-153">When you run this image in your container, it will automatically start IIS and installed websites.</span></span>

<span data-ttu-id="55344-154">Il documento Dockerfile che crea l'immagine è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="55344-154">The Dockerfile that creates your image looks like this:</span></span>

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

<span data-ttu-id="55344-155">Non esiste alcun comando `ENTRYPOINT` in questo Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="55344-155">There is no `ENTRYPOINT` command in this Dockerfile.</span></span> <span data-ttu-id="55344-156">Non è necessario.</span><span class="sxs-lookup"><span data-stu-id="55344-156">You don't need one.</span></span> <span data-ttu-id="55344-157">Quando si esegue Windows Server con IIS, il processo IIS è il punto di ingresso, è configurato per avviarsi nell'immagine di base aspnet.</span><span class="sxs-lookup"><span data-stu-id="55344-157">When running Windows Server with IIS, the IIS process is the entrypoint, which is configured to start in the aspnet base image.</span></span>

<span data-ttu-id="55344-158">Eseguire il comando di compilazione di Docker per creare l'immagine che esegue l'app ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="55344-158">Run the Docker build command to create the image that runs your ASP.NET app.</span></span> <span data-ttu-id="55344-159">A tale scopo, aprire una finestra di PowerShell nella directory del progetto e digitare il comando seguente nella directory della soluzione:</span><span class="sxs-lookup"><span data-stu-id="55344-159">To do this, open a PowerShell window in the directory of your project and type the following command in the solution directory:</span></span>

```console
docker build -t mvcrandomanswers .
```

<span data-ttu-id="55344-160">Questo comando creerà una nuova immagine con le istruzioni del Dockerfile, denominazione (-t tag) dell'immagine come mvcrandomanswers.</span><span class="sxs-lookup"><span data-stu-id="55344-160">This command will build the new image using the instructions in your Dockerfile, naming (-t tagging) the image as mvcrandomanswers.</span></span> <span data-ttu-id="55344-161">L'operazione potrebbe includere il pull dell'immagine di base dall'[hub Docker](http://hub.docker.com) e l'aggiunta dell'app a tale immagine.</span><span class="sxs-lookup"><span data-stu-id="55344-161">This may include pulling the base image from [Docker Hub](http://hub.docker.com), and then adding your app to that image.</span></span>

<span data-ttu-id="55344-162">Dopo aver completato il comando è possibile eseguire il comando `docker images` per visualizzare informazioni sulla nuova immagine:</span><span class="sxs-lookup"><span data-stu-id="55344-162">Once that command completes, you can run the `docker images` command to see information on the new image:</span></span>

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

<span data-ttu-id="55344-163">L'ID immagine sarà diverso nel computer in uso.</span><span class="sxs-lookup"><span data-stu-id="55344-163">The IMAGE ID will be different on your machine.</span></span> <span data-ttu-id="55344-164">A questo punto è possibile eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="55344-164">Now, let's run the app.</span></span>

## <a name="start-a-container"></a><span data-ttu-id="55344-165">Avviare un contenitore</span><span class="sxs-lookup"><span data-stu-id="55344-165">Start a container</span></span>

<span data-ttu-id="55344-166">Avviare un contenitore eseguendo il comando `docker run` seguente:</span><span class="sxs-lookup"><span data-stu-id="55344-166">Start a container by executing the following `docker run` command:</span></span>

```console
docker run -d --name randomanswers mvcrandomanswers
```

<span data-ttu-id="55344-167">L'argomento `-d` indica a Docker di avviare l'immagine senza collegamento.</span><span class="sxs-lookup"><span data-stu-id="55344-167">The `-d` argument tells Docker to start the image in detached mode.</span></span> <span data-ttu-id="55344-168">Ciò significa che l'immagine Docker viene eseguita scollegata dalla shell corrente.</span><span class="sxs-lookup"><span data-stu-id="55344-168">That means the Docker image runs disconnected from the current shell.</span></span>

<span data-ttu-id="55344-169">In molti esempi di docker, si può vedere -p per il mapping delle porte contenitore e host.</span><span class="sxs-lookup"><span data-stu-id="55344-169">In many docker examples, you may see -p to map the container and host ports.</span></span> <span data-ttu-id="55344-170">L'immagine aspnet predefinita è già configurato il contenitore per l'ascolto sulla porta 80 ed esporre quest'ultimo.</span><span class="sxs-lookup"><span data-stu-id="55344-170">The default aspnet image has already configured the container to listen on port 80 and expose it.</span></span> 

<span data-ttu-id="55344-171">Il valore `--name randomanswers` assegna un nome al contenitore in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="55344-171">The `--name randomanswers` gives a name to the running container.</span></span> <span data-ttu-id="55344-172">È possibile usare questo nome anziché l'ID del contenitore nella maggior parte dei comandi.</span><span class="sxs-lookup"><span data-stu-id="55344-172">You can use this name instead of the container ID in most commands.</span></span>

<span data-ttu-id="55344-173">Il valore `mvcrandomanswers` è il nome dell'immagine da avviare.</span><span class="sxs-lookup"><span data-stu-id="55344-173">The `mvcrandomanswers` is the name of the image to start.</span></span>

## <a name="verify-in-the-browser"></a><span data-ttu-id="55344-174">Verificare nel browser</span><span class="sxs-lookup"><span data-stu-id="55344-174">Verify in the browser</span></span>

> [!NOTE]
> <span data-ttu-id="55344-175">Con la versione corrente di contenitore di Windows, è possibile individuare `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="55344-175">With the current Windows Container release, you can't browse to `http://localhost`.</span></span>
> <span data-ttu-id="55344-176">Questo è un comportamento noto in WinNAT e verrà risolto in futuro.</span><span class="sxs-lookup"><span data-stu-id="55344-176">This is a known behavior in WinNAT, and it will be resolved in the future.</span></span> <span data-ttu-id="55344-177">Fino a quel momento sarà necessario usare l'indirizzo IP del contenitore.</span><span class="sxs-lookup"><span data-stu-id="55344-177">Until that is addressed, you need to use the IP address of the container.</span></span>

<span data-ttu-id="55344-178">Dopo l'avvio del contenitore, trovarne l'indirizzo IP in modo da consentire la connessione al contenitore in esecuzione da un browser:</span><span class="sxs-lookup"><span data-stu-id="55344-178">Once the container starts, find its IP address so that you can connect to your running container from a browser:</span></span>

```console
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" randomanswers
172.31.194.61
```

<span data-ttu-id="55344-179">Connettersi al contenitore in esecuzione utilizzando l'indirizzo IPv4, `http://172.31.194.61` nell'esempio illustrato.</span><span class="sxs-lookup"><span data-stu-id="55344-179">Connect to the running container using the IPv4 address, `http://172.31.194.61` in the example shown.</span></span> <span data-ttu-id="55344-180">Digitando l'URL nel browser viene visualizzato il sito in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="55344-180">Type that URL into your browser, and you should see the running site.</span></span>

> [!NOTE]
> <span data-ttu-id="55344-181">Alcuni software VPN o proxy possono impedire la navigazione nel sito.</span><span class="sxs-lookup"><span data-stu-id="55344-181">Some VPN or proxy software may prevent you from navigating to your site.</span></span>
> <span data-ttu-id="55344-182">È possibile disabilitarli temporaneamente per verificare che il contenitore funzioni.</span><span class="sxs-lookup"><span data-stu-id="55344-182">You can temporarily disable it to make sure your container is working.</span></span>

<span data-ttu-id="55344-183">La directory di esempio su GitHub contiene un [script di PowerShell](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator/run.ps1) che esegue automaticamente questi comandi.</span><span class="sxs-lookup"><span data-stu-id="55344-183">The sample directory on GitHub contains a [PowerShell script](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator/run.ps1) that executes these commands for you.</span></span> <span data-ttu-id="55344-184">Aprire una finestra di PowerShell, passare alla directory soluzione e digitare:</span><span class="sxs-lookup"><span data-stu-id="55344-184">Open a PowerShell window, change directory to your solution directory, and type:</span></span>

```console
./run.ps1
```

<span data-ttu-id="55344-185">Il comando precedente crea l'immagine, visualizza l'elenco di immagini presenti nel computer, avvia un contenitore e visualizza l'indirizzo IP per tale contenitore.</span><span class="sxs-lookup"><span data-stu-id="55344-185">The command above builds the image, displays the list of images on your machine, starts a container, and displays the IP address for that container.</span></span>

<span data-ttu-id="55344-186">Per arrestare il contenitore, eseguire un comando `docker
stop`:</span><span class="sxs-lookup"><span data-stu-id="55344-186">To stop your container, issue a `docker
stop` command:</span></span>

```console
docker stop randomanswers
```

<span data-ttu-id="55344-187">Per rimuovere il contenitore, eseguire un comando `docker rm`:</span><span class="sxs-lookup"><span data-stu-id="55344-187">To remove the container, issue a `docker rm` command:</span></span>

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Passare a un contenitore di Windows"
[publish-connection]: media/aspnetmvc/PublishConnection.png "Pubblicare nel file system"
[publish-settings]: media/aspnetmvc/PublishSettings.png "Impostazioni di pubblicazione"
