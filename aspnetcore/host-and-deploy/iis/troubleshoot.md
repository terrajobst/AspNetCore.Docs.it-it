---
title: Risolvere i problemi di ASP.NET Core in IIS
author: guardrex
description: Informazioni su come diagnosticare i problemi delle distribuzioni Internet Information Services (IIS) di app ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 4df370dd9b1a5a651bcf767b8b9ace4220bdc345
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313648"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="d556b-103">Risolvere i problemi di ASP.NET Core in IIS</span><span class="sxs-lookup"><span data-stu-id="d556b-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="d556b-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d556b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d556b-105">In questo articolo vengono fornite istruzioni su come diagnosticare un problema di avvio di un'app ASP.NET Core durante l'hosting con [Internet Information Services (IIS)](/iis).</span><span class="sxs-lookup"><span data-stu-id="d556b-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="d556b-106">Le informazioni in questo articolo si applicano all'hosting in IIS su Windows Server e Windows Desktop.</span><span class="sxs-lookup"><span data-stu-id="d556b-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

<span data-ttu-id="d556b-107">Altri argomenti sulla risoluzione dei problemi:</span><span class="sxs-lookup"><span data-stu-id="d556b-107">Additional troubleshooting topics:</span></span>

* <span data-ttu-id="d556b-108">Servizio app di Azure usa anche il [modulo di ASP.NET Core](xref:host-and-deploy/aspnet-core-module) e IIS per ospitare le app.</span><span class="sxs-lookup"><span data-stu-id="d556b-108">Azure App Service also uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and IIS to host apps.</span></span> <span data-ttu-id="d556b-109">Per consigli per la risoluzione dei problemi relativi in particolare a Servizio app di Azure, vedere <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="d556b-109">For troubleshooting advice that pertains specifically to Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>
* <span data-ttu-id="d556b-110"><xref:fundamentals/error-handling> offre informazioni su come gestire gli errori nelle app ASP.NET Core durante lo sviluppo in un sistema locale.</span><span class="sxs-lookup"><span data-stu-id="d556b-110"><xref:fundamentals/error-handling> covers how to handle errors in ASP.NET Core apps during development on a local system.</span></span>
* <span data-ttu-id="d556b-111">[Informazioni su come eseguire il debug con Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) presenta le funzionalità del debugger di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d556b-111">[Learn to debug using Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) introduces the features of the Visual Studio debugger.</span></span>
* <span data-ttu-id="d556b-112">[Debug con Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) descrive il supporto del debug incorporato in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d556b-112">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) describes the debugging support built into Visual Studio Code.</span></span>

[!INCLUDE[](~/includes/azure-iis-startup-errors.md)]

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="d556b-113">Risolvere gli errori di avvio delle app</span><span class="sxs-lookup"><span data-stu-id="d556b-113">Troubleshoot app startup errors</span></span>

### <a name="enable-the-aspnet-core-module-debug-log"></a><span data-ttu-id="d556b-114">Abilitare il log di debug del modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d556b-114">Enable the ASP.NET Core Module debug log</span></span>

<span data-ttu-id="d556b-115">Aggiungere le impostazioni del gestore seguenti al file dell'app *web.config* per abilitare i log di debug del modulo ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="d556b-115">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug logs:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="d556b-116">Verificare che il percorso specificato per il log esista e che l'identità del pool di applicazioni abbia le autorizzazioni di scrittura nel percorso.</span><span class="sxs-lookup"><span data-stu-id="d556b-116">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

### <a name="application-event-log"></a><span data-ttu-id="d556b-117">Registro eventi dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="d556b-117">Application Event Log</span></span>

<span data-ttu-id="d556b-118">Accedere al log eventi dell'applicazione:</span><span class="sxs-lookup"><span data-stu-id="d556b-118">Access the Application Event Log:</span></span>

1. <span data-ttu-id="d556b-119">Aprire il menu Start, cercare **Visualizzatore eventi** e quindi selezionare l'app **Visualizzatore eventi**.</span><span class="sxs-lookup"><span data-stu-id="d556b-119">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="d556b-120">In **Visualizzatore eventi** aprire il nodo **Registri di Windows**.</span><span class="sxs-lookup"><span data-stu-id="d556b-120">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="d556b-121">Selezionare **Applicazione** per aprire il log eventi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d556b-121">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="d556b-122">Cercare gli errori associati all'app in cui si è verificato il problema.</span><span class="sxs-lookup"><span data-stu-id="d556b-122">Search for errors associated with the failing app.</span></span> <span data-ttu-id="d556b-123">Gli errori presentano un valore *Modulo AspNetCore IIS* o *Modulo AspNetCore IIS Express* nella colonna *Origine*.</span><span class="sxs-lookup"><span data-stu-id="d556b-123">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="d556b-124">Eseguire l'app da un prompt dei comandi</span><span class="sxs-lookup"><span data-stu-id="d556b-124">Run the app at a command prompt</span></span>

<span data-ttu-id="d556b-125">Molti errori di avvio non producono informazioni utili nel log eventi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d556b-125">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="d556b-126">È possibile individuare la causa di alcuni errori eseguendo l'app da un prompt dei comandi nel sistema host.</span><span class="sxs-lookup"><span data-stu-id="d556b-126">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="d556b-127">Distribuzione dipendente dal framework</span><span class="sxs-lookup"><span data-stu-id="d556b-127">Framework-dependent deployment</span></span>

<span data-ttu-id="d556b-128">Se l'app è una [distribuzione dipendente dal framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="d556b-128">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="d556b-129">Da un prompt dei comandi passare alla cartella di distribuzione e avviare l'app eseguendo l'assembly dell'app con *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="d556b-129">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="d556b-130">Nel comando seguente sostituire \<assembly_name> con il nome dell'assembly dell'app: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="d556b-130">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="d556b-131">L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà scritto nella finestra della console.</span><span class="sxs-lookup"><span data-stu-id="d556b-131">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="d556b-132">Se gli errori si verificano quando si effettua una richiesta all'app, effettuare una richiesta all'host e alla porta su cui è in ascolto Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d556b-132">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="d556b-133">Usando l'host e la porta predefiniti, effettuare una richiesta a `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="d556b-133">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="d556b-134">Se l'app risponde normalmente nell'indirizzo endpoint di Kestrel, è più probabile che il problema sia associato alla configurazione dell'host e che non sia interno all'app.</span><span class="sxs-lookup"><span data-stu-id="d556b-134">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="d556b-135">Distribuzione autonoma</span><span class="sxs-lookup"><span data-stu-id="d556b-135">Self-contained deployment</span></span>

<span data-ttu-id="d556b-136">Se l'app è una [distribuzione autonoma](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="d556b-136">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="d556b-137">Da un prompt dei comandi passare alla cartella di distribuzione e avviare il file eseguibile dell'app.</span><span class="sxs-lookup"><span data-stu-id="d556b-137">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="d556b-138">Nel comando seguente sostituire \<assembly_name> con il nome dell'assembly dell'app: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="d556b-138">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="d556b-139">L'output della console per l'app, in cui sono indicati gli eventuali errori, verrà scritto nella finestra della console.</span><span class="sxs-lookup"><span data-stu-id="d556b-139">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="d556b-140">Se gli errori si verificano quando si effettua una richiesta all'app, effettuare una richiesta all'host e alla porta su cui è in ascolto Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d556b-140">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="d556b-141">Usando l'host e la porta predefiniti, effettuare una richiesta a `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="d556b-141">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="d556b-142">Se l'app risponde normalmente nell'indirizzo endpoint di Kestrel, è più probabile che il problema sia associato alla configurazione dell'host e che non sia interno all'app.</span><span class="sxs-lookup"><span data-stu-id="d556b-142">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="d556b-143">Log stdout del modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d556b-143">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="d556b-144">Per abilitare e visualizzare i log stdout:</span><span class="sxs-lookup"><span data-stu-id="d556b-144">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="d556b-145">Passare alla cartella di distribuzione del sito nel sistema host.</span><span class="sxs-lookup"><span data-stu-id="d556b-145">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="d556b-146">Se la cartella *logs* non è presente, creare la cartella.</span><span class="sxs-lookup"><span data-stu-id="d556b-146">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="d556b-147">Per istruzioni su come impostare MSBuild per la creazione automatica della cartella *logs* nella distribuzione, vedere l'argomento [Struttura della directory](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="d556b-147">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="d556b-148">Modificare il file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="d556b-148">Edit the *web.config* file.</span></span> <span data-ttu-id="d556b-149">Impostare **stdoutLogEnabled** su `true` e modificare il percorso di **stdoutLogFile** in modo da fare riferimento alla cartella *logs*, ad esempio `.\logs\stdout`.</span><span class="sxs-lookup"><span data-stu-id="d556b-149">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="d556b-150">`stdout` nel percorso è il prefisso del nome del file di log.</span><span class="sxs-lookup"><span data-stu-id="d556b-150">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="d556b-151">Un timestamp, l'ID del processo e l'estensione del file vengono aggiunti automaticamente al momento della creazione del log.</span><span class="sxs-lookup"><span data-stu-id="d556b-151">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="d556b-152">Usando `stdout` come prefisso del nome del file, un tipico file di log è denominato *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="d556b-152">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="d556b-153">Assicurarsi che l'identità del pool di applicazioni disponga delle autorizzazioni di scrittura per la cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="d556b-153">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="d556b-154">Salvare il file *web.config* aggiornato.</span><span class="sxs-lookup"><span data-stu-id="d556b-154">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="d556b-155">Effettuare una richiesta all'app.</span><span class="sxs-lookup"><span data-stu-id="d556b-155">Make a request to the app.</span></span>
1. <span data-ttu-id="d556b-156">Passare alla cartella *logs*.</span><span class="sxs-lookup"><span data-stu-id="d556b-156">Navigate to the *logs* folder.</span></span> <span data-ttu-id="d556b-157">Trovare e aprire il log stdout più recente.</span><span class="sxs-lookup"><span data-stu-id="d556b-157">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="d556b-158">Esaminare il log per verificare se sono presenti errori.</span><span class="sxs-lookup"><span data-stu-id="d556b-158">Study the log for errors.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d556b-159">Al termine della risoluzione dei problemi, disabilitare la registrazione stdout.</span><span class="sxs-lookup"><span data-stu-id="d556b-159">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="d556b-160">Modificare il file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="d556b-160">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="d556b-161">Impostare **stdoutLogEnabled** su `false`.</span><span class="sxs-lookup"><span data-stu-id="d556b-161">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="d556b-162">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="d556b-162">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="d556b-163">La mancata disabilitazione del log stdout può causare un errore dell'app o del server.</span><span class="sxs-lookup"><span data-stu-id="d556b-163">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="d556b-164">Non esiste alcun limite per le dimensioni dei file di log o il numero di file di log che è possibile creare.</span><span class="sxs-lookup"><span data-stu-id="d556b-164">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="d556b-165">Per la registrazione di routine in un'app ASP.NET Core, usare una libreria di registrazione che limita le dimensioni dei file di log e ne esegue la rotazione.</span><span class="sxs-lookup"><span data-stu-id="d556b-165">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="d556b-166">Per altre informazioni, vedere [Provider di registrazione di terze parti](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="d556b-166">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enable-the-developer-exception-page"></a><span data-ttu-id="d556b-167">Abilitare la pagina delle eccezioni per gli sviluppatori</span><span class="sxs-lookup"><span data-stu-id="d556b-167">Enable the Developer Exception Page</span></span>

<span data-ttu-id="d556b-168">La variabile di ambiente `ASPNETCORE_ENVIRONMENT` [ può essere aggiunta a web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) per eseguire l'app nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="d556b-168">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="d556b-169">Purché l'ambiente non sia sottoposto a override durante l'avvio dell'app tramite `UseEnvironment` nel generatore di host, l'impostazione della variabile di ambiente consente di visualizzare la [pagina delle eccezioni per gli sviluppatori](xref:fundamentals/error-handling) quando viene eseguita l'app.</span><span class="sxs-lookup"><span data-stu-id="d556b-169">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

<span data-ttu-id="d556b-170">L'impostazione della variabile di ambiente per `ASPNETCORE_ENVIRONMENT` è consigliata solo per l'uso in server di gestione temporanea e test non esposti a Internet.</span><span class="sxs-lookup"><span data-stu-id="d556b-170">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="d556b-171">Al termine della risoluzione dei problemi, rimuovere la variabile di ambiente dal file *web.config*.</span><span class="sxs-lookup"><span data-stu-id="d556b-171">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="d556b-172">Per informazioni sull'impostazione delle variabili di ambiente in *web.config*, vedere [elemento figlio environmentVariables di aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="d556b-172">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="d556b-173">Errori di avvio comuni</span><span class="sxs-lookup"><span data-stu-id="d556b-173">Common startup errors</span></span>

<span data-ttu-id="d556b-174">Vedere <xref:host-and-deploy/azure-iis-errors-reference>.</span><span class="sxs-lookup"><span data-stu-id="d556b-174">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="d556b-175">Nell'argomento di riferimento è descritta la maggior parte dei problemi comuni che impediscono l'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="d556b-175">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="d556b-176">Ottenere dati da un'app</span><span class="sxs-lookup"><span data-stu-id="d556b-176">Obtain data from an app</span></span>

<span data-ttu-id="d556b-177">Se un'app è in grado di rispondere alle richieste, è possibile ottenere dati sulle richieste, le connessioni e altri dati dall'app tramite middleware inline di terminale.</span><span class="sxs-lookup"><span data-stu-id="d556b-177">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="d556b-178">Per altre informazioni e codice di esempio, vedere <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="d556b-178">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

## <a name="create-a-dump"></a><span data-ttu-id="d556b-179">Creare un dump</span><span class="sxs-lookup"><span data-stu-id="d556b-179">Create a dump</span></span>

<span data-ttu-id="d556b-180">Un *dump* è uno snapshot della memoria del sistema e può essere utile per determinare la causa di un arresto anomalo dell'app, un errore di avvio o un'app lenta.</span><span class="sxs-lookup"><span data-stu-id="d556b-180">A *dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="d556b-181">Arresto anomalo o eccezione di un'app</span><span class="sxs-lookup"><span data-stu-id="d556b-181">App crashes or encounters an exception</span></span>

<span data-ttu-id="d556b-182">Ottenere e analizzare un dump da [Segnalazione errori Windows](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="d556b-182">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="d556b-183">Creare una cartella per i file dump di arresto anomalo del sistema in `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="d556b-183">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="d556b-184">Il pool di app deve avere accesso in scrittura alla cartella.</span><span class="sxs-lookup"><span data-stu-id="d556b-184">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="d556b-185">Eseguire lo [script di PowerShell EnableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="d556b-185">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="d556b-186">Se l'app usa il [modello di hosting in-process](xref:host-and-deploy/iis/index#in-process-hosting-model), eseguire lo script per *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="d556b-186">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="d556b-187">Se l'app usa il [modello di hosting out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), eseguire lo script per *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="d556b-187">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="d556b-188">Eseguire l'app nelle condizioni che causano l'arresto anomalo.</span><span class="sxs-lookup"><span data-stu-id="d556b-188">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="d556b-189">Dopo che si è verificato l'arresto anomalo, eseguire lo [script di PowerShell DisableDumps](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="d556b-189">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="d556b-190">Se l'app usa il [modello di hosting in-process](xref:host-and-deploy/iis/index#in-process-hosting-model), eseguire lo script per *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="d556b-190">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="d556b-191">Se l'app usa il [modello di hosting out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model), eseguire lo script per *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="d556b-191">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="d556b-192">Dopo l'arresto anomalo di un'app e la raccolta dei dump, l'app può terminare normalmente.</span><span class="sxs-lookup"><span data-stu-id="d556b-192">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="d556b-193">Lo script di PowerShell configura Segnalazione errori Windows per raccogliere fino a cinque dump per ogni app.</span><span class="sxs-lookup"><span data-stu-id="d556b-193">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="d556b-194">I dump di arresto anomalo del sistema potrebbero richiedere una grande quantità di spazio su disco (fino a diversi gigabyte ognuno).</span><span class="sxs-lookup"><span data-stu-id="d556b-194">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="d556b-195">L'app si blocca, si verifica un errore durante l'avvio o viene eseguita normalmente</span><span class="sxs-lookup"><span data-stu-id="d556b-195">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="d556b-196">Quando un'app si *blocca* (smette di rispondere ma senza arresto anomalo), si verifica un errore durante l'avvio o viene eseguita normalmente, vedere [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) (File dump in modalità utente: scelta dello strumento migliore) per selezionare uno strumento appropriato per produrre il dump.</span><span class="sxs-lookup"><span data-stu-id="d556b-196">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

### <a name="analyze-the-dump"></a><span data-ttu-id="d556b-197">Analizzare il dump</span><span class="sxs-lookup"><span data-stu-id="d556b-197">Analyze the dump</span></span>

<span data-ttu-id="d556b-198">È possibile analizzare un dump usando diversi approcci.</span><span class="sxs-lookup"><span data-stu-id="d556b-198">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="d556b-199">Per altre informazioni, vedere [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file) (Analisi di un file dump in modalità utente).</span><span class="sxs-lookup"><span data-stu-id="d556b-199">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="d556b-200">Debug remoto</span><span class="sxs-lookup"><span data-stu-id="d556b-200">Remote debugging</span></span>

<span data-ttu-id="d556b-201">Vedere [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) (Eseguire il debug remoto di ASP.NET Core in un computer remoto con IIS in Visual Studio 2017) nella documentazione di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d556b-201">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="d556b-202">Application Insights</span><span class="sxs-lookup"><span data-stu-id="d556b-202">Application Insights</span></span>

<span data-ttu-id="d556b-203">[Application Insights](/azure/application-insights/) fornisce dati di telemetria dalle app ospitate da IIS, inclusi gli errori di registrazione e le funzionalità di creazione di report.</span><span class="sxs-lookup"><span data-stu-id="d556b-203">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="d556b-204">Application Insights può segnalare solo gli errori che si verificano dopo l'avvio dell'app, quando diventano disponibili le funzionalità di registrazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="d556b-204">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="d556b-205">Per altre informazioni, vedere [Application Insights per ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="d556b-205">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-advice"></a><span data-ttu-id="d556b-206">Suggerimenti aggiuntivi</span><span class="sxs-lookup"><span data-stu-id="d556b-206">Additional advice</span></span>

<span data-ttu-id="d556b-207">Talvolta in un'app funzionante si verifica un problema subito dopo l'aggiornamento di .NET Core SDK nelle versioni computer o pacchetto di sviluppo all'interno dell'app.</span><span class="sxs-lookup"><span data-stu-id="d556b-207">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="d556b-208">In alcuni casi i pacchetti incoerenti possono interrompere un'app quando si eseguono aggiornamenti principali.</span><span class="sxs-lookup"><span data-stu-id="d556b-208">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="d556b-209">La maggior parte di questi problemi può essere risolta attenendosi alle istruzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="d556b-209">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="d556b-210">Eliminare le cartelle *bin* e *obj*.</span><span class="sxs-lookup"><span data-stu-id="d556b-210">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="d556b-211">Cancellare le cache dei pacchetti in *%UserProfile%\\.nuget\\packages* e *%LocalAppData%\\Nuget\\v3-cache*.</span><span class="sxs-lookup"><span data-stu-id="d556b-211">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="d556b-212">Ripristinare e ricompilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="d556b-212">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="d556b-213">Verificare che la distribuzione precedente nel server sia stata completamente eliminata prima di ridistribuire l'app.</span><span class="sxs-lookup"><span data-stu-id="d556b-213">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="d556b-214">Un modo pratico per cancellare le cache dei pacchetti consiste nell'eseguire `dotnet nuget locals all --clear` da un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="d556b-214">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
>
> <span data-ttu-id="d556b-215">La cancellazione delle cache dei pacchetti può anche essere eseguita usando lo strumento [nuget.exe](https://www.nuget.org/downloads) ed eseguendo il comando `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="d556b-215">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="d556b-216">*nuget.exe* non è un'installazione inclusa con il sistema operativo desktop Windows e deve essere ottenuta separatamente dal [sito Web NuGet](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="d556b-216">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d556b-217">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d556b-217">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
