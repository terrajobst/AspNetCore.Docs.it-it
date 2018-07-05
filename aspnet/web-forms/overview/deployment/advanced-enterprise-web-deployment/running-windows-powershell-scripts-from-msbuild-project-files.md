---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Esecuzione degli script di Windows PowerShell dai file di progetto MSBuild | Microsoft Docs
author: jrjlee
description: Questo argomento descrive come eseguire uno script di Windows PowerShell come parte di un processo di compilazione e distribuzione. È possibile eseguire uno script in locale (in altre parole, in b...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: faedcee480b6c50dc560055206fedbe7af4d5f67
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803149"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a><span data-ttu-id="aca44-104">Esecuzione degli script di Windows PowerShell dai file di progetto MSBuild</span><span class="sxs-lookup"><span data-stu-id="aca44-104">Running Windows PowerShell Scripts from MSBuild Project Files</span></span>
====================
<span data-ttu-id="aca44-105">da [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="aca44-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="aca44-106">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="aca44-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="aca44-107">Questo argomento descrive come eseguire uno script di Windows PowerShell come parte di un processo di compilazione e distribuzione.</span><span class="sxs-lookup"><span data-stu-id="aca44-107">This topic describes how to run a Windows PowerShell script as part of a build and deployment process.</span></span> <span data-ttu-id="aca44-108">È possibile eseguire uno script in locale (in altre parole, nel server di compilazione) o in modalità remota, ad esempio in un server di database o un server web di destinazione.</span><span class="sxs-lookup"><span data-stu-id="aca44-108">You can run a script locally (in other words, on the build server) or remotely, like on a destination web server or database server.</span></span>
> 
> <span data-ttu-id="aca44-109">Esistono molti motivi per cui si potrebbe voler eseguire uno script di Windows PowerShell di post-distribuzione.</span><span class="sxs-lookup"><span data-stu-id="aca44-109">There are lots of reasons why you might want to run a post-deployment Windows PowerShell script.</span></span> <span data-ttu-id="aca44-110">Può, ad esempio, essere necessario:</span><span class="sxs-lookup"><span data-stu-id="aca44-110">For example, you might want to:</span></span>
> 
> - <span data-ttu-id="aca44-111">Aggiungere un'origine evento personalizzato nel Registro di sistema.</span><span class="sxs-lookup"><span data-stu-id="aca44-111">Add a custom event source to the registry.</span></span>
> - <span data-ttu-id="aca44-112">Generare una directory del file system per i caricamenti.</span><span class="sxs-lookup"><span data-stu-id="aca44-112">Generate a file system directory for uploads.</span></span>
> - <span data-ttu-id="aca44-113">Eliminare le directory di compilazione.</span><span class="sxs-lookup"><span data-stu-id="aca44-113">Clean up build directories.</span></span>
> - <span data-ttu-id="aca44-114">Scrivere le voci in un file di log personalizzato.</span><span class="sxs-lookup"><span data-stu-id="aca44-114">Write entries to a custom log file.</span></span>
> - <span data-ttu-id="aca44-115">Inviare messaggi di posta elettronica che invita gli utenti a un'applicazione web appena sottoposti a provisioning.</span><span class="sxs-lookup"><span data-stu-id="aca44-115">Send emails inviting users to a newly provisioned web application.</span></span>
> - <span data-ttu-id="aca44-116">Creare account utente con le autorizzazioni appropriate.</span><span class="sxs-lookup"><span data-stu-id="aca44-116">Create user accounts with the appropriate permissions.</span></span>
> - <span data-ttu-id="aca44-117">Configurare la replica tra istanze di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="aca44-117">Configure replication between SQL Server instances.</span></span>
> 
> <span data-ttu-id="aca44-118">In questo argomento illustrerà come eseguire gli script di Windows PowerShell in locale e in remoto da una destinazione personalizzata in un file di progetto di Microsoft Build Engine (MSBuild).</span><span class="sxs-lookup"><span data-stu-id="aca44-118">This topic will show you how to run Windows PowerShell scripts both locally and remotely from a custom target in a Microsoft Build Engine (MSBuild) project file.</span></span>


<span data-ttu-id="aca44-119">In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione aziendale di una società fittizia, denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.</span><span class="sxs-lookup"><span data-stu-id="aca44-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="aca44-120">Il metodo di distribuzione il fulcro delle esercitazioni seguenti si basa sull'approccio di file di progetto split descritto in [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in cui il processo di compilazione è controllato da due file di progetto&#x2014;contenente una istruzioni che si applicano a tutti gli ambienti di destinazione e quella che contiene le impostazioni di compilazione e distribuzione specifiche dell'ambiente di creazione.</span><span class="sxs-lookup"><span data-stu-id="aca44-120">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="aca44-121">In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.</span><span class="sxs-lookup"><span data-stu-id="aca44-121">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="aca44-122">Panoramica di Task</span><span class="sxs-lookup"><span data-stu-id="aca44-122">Task Overview</span></span>

<span data-ttu-id="aca44-123">Per eseguire uno script di Windows PowerShell come parte di un processo di distribuzione automatica o passo a passo, è necessario completare queste attività di alto livello:</span><span class="sxs-lookup"><span data-stu-id="aca44-123">To run a Windows PowerShell script as part of an automated or single-step deployment process, you'll need to complete these high-level tasks:</span></span>

- <span data-ttu-id="aca44-124">Aggiungere lo script di Windows PowerShell per la soluzione e al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="aca44-124">Add the Windows PowerShell script to your solution and to source control.</span></span>
- <span data-ttu-id="aca44-125">Creare un comando che richiama lo script di Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="aca44-125">Create a command that invokes your Windows PowerShell script.</span></span>
- <span data-ttu-id="aca44-126">Eseguire l'escape di tutti i caratteri XML riservati nel comando.</span><span class="sxs-lookup"><span data-stu-id="aca44-126">Escape any reserved XML characters in your command.</span></span>
- <span data-ttu-id="aca44-127">Creare una destinazione nel file di progetto MSBuild personalizzato e usare la **Exec** attività per eseguire il comando.</span><span class="sxs-lookup"><span data-stu-id="aca44-127">Create a target in your custom MSBuild project file and use the **Exec** task to run your command.</span></span>

<span data-ttu-id="aca44-128">In questo argomento illustrerà come eseguire queste procedure.</span><span class="sxs-lookup"><span data-stu-id="aca44-128">This topic will show you how to perform these procedures.</span></span> <span data-ttu-id="aca44-129">Le attività e procedure dettagliate in questo argomento presuppongono che abbia già familiarità con le proprietà e destinazioni MSBuild e di comprendere come usare un file di progetto MSBuild personalizzato per gestire un processo di compilazione e distribuzione.</span><span class="sxs-lookup"><span data-stu-id="aca44-129">The tasks and walkthroughs in this topic assume that you're already familiar with MSBuild targets and properties, and that you understand how to use a custom MSBuild project file to drive a build and deployment process.</span></span> <span data-ttu-id="aca44-130">Per altre informazioni, vedere [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="aca44-130">For more information, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

## <a name="creating-and-adding-windows-powershell-scripts"></a><span data-ttu-id="aca44-131">Creazione e l'aggiunta degli script di Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="aca44-131">Creating and Adding Windows PowerShell Scripts</span></span>

<span data-ttu-id="aca44-132">Le attività in questo argomento utilizzano uno script di Windows PowerShell di esempio denominato **LogDeploy.ps1** per illustrare come eseguire gli script da MSBuild.</span><span class="sxs-lookup"><span data-stu-id="aca44-132">The tasks in this topic use a sample Windows PowerShell script named **LogDeploy.ps1** to illustrate how to run scripts from MSBuild.</span></span> <span data-ttu-id="aca44-133">Il **LogDeploy.ps1** script contiene una funzione semplice che scrive una voce a riga singola in un file di log:</span><span class="sxs-lookup"><span data-stu-id="aca44-133">The **LogDeploy.ps1** script contains a simple function that writes a single-line entry to a log file:</span></span>


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.ps1)]


<span data-ttu-id="aca44-134">Il **LogDeploy.ps1** script accetta due parametri.</span><span class="sxs-lookup"><span data-stu-id="aca44-134">The **LogDeploy.ps1** script accepts two parameters.</span></span> <span data-ttu-id="aca44-135">Il primo parametro rappresenta il percorso completo al file di log a cui si desidera aggiungere una voce e il secondo parametro rappresenta la destinazione di distribuzione che si desidera registrare nel file di log.</span><span class="sxs-lookup"><span data-stu-id="aca44-135">The first parameter represents the full path to the log file to which you want to add an entry, and the second parameter represents the deployment destination that you want to record in the log file.</span></span> <span data-ttu-id="aca44-136">Quando si esegue lo script, aggiunge una riga nel file di log nel formato seguente:</span><span class="sxs-lookup"><span data-stu-id="aca44-136">When you run the script, it adds a line to the log file in this format:</span></span>


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


<span data-ttu-id="aca44-137">Per rendere il **LogDeploy.ps1** script disponibili per MSBuild, è necessario:</span><span class="sxs-lookup"><span data-stu-id="aca44-137">To make the **LogDeploy.ps1** script available to MSBuild, you need to:</span></span>

- <span data-ttu-id="aca44-138">Aggiungere lo script al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="aca44-138">Add the script to source control.</span></span>
- <span data-ttu-id="aca44-139">Aggiungere lo script alla soluzione in Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="aca44-139">Add the script to your solution in Visual Studio 2010.</span></span>

<span data-ttu-id="aca44-140">Non è necessario distribuire lo script con il contenuto della soluzione, indipendentemente dal fatto se si prevede di eseguire lo script nel server di compilazione o in un computer remoto.</span><span class="sxs-lookup"><span data-stu-id="aca44-140">You don't need to deploy the script with your solution content, regardless of whether you plan to run the script on the build server or on a remote computer.</span></span> <span data-ttu-id="aca44-141">Una possibilità consiste nell'aggiungere lo script per una cartella della soluzione.</span><span class="sxs-lookup"><span data-stu-id="aca44-141">One option is to add the script to a solution folder.</span></span> <span data-ttu-id="aca44-142">Nell'esempio Contact Manager, poiché si desidera usare lo script di Windows PowerShell come parte del processo di distribuzione, è opportuno aggiungere lo script alla cartella della soluzione di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="aca44-142">In the Contact Manager example, because you want to use the Windows PowerShell script as part of the deployment process, it makes sense to add the script to the Publish solution folder.</span></span>

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

<span data-ttu-id="aca44-143">Il contenuto di cartelle della soluzione viene copiato per server di compilazione come materiale di origine.</span><span class="sxs-lookup"><span data-stu-id="aca44-143">The contents of solution folders are copied to build servers as source material.</span></span> <span data-ttu-id="aca44-144">Tuttavia, non costituiscono nessuna parte di qualsiasi output del progetto.</span><span class="sxs-lookup"><span data-stu-id="aca44-144">However, they form no part of any project output.</span></span>

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a><span data-ttu-id="aca44-145">L'esecuzione di uno Script di Windows PowerShell nel Server di compilazione</span><span class="sxs-lookup"><span data-stu-id="aca44-145">Executing a Windows PowerShell Script on the Build Server</span></span>

<span data-ttu-id="aca44-146">In alcuni scenari, è possibile eseguire gli script di Windows PowerShell nel computer che compila i progetti.</span><span class="sxs-lookup"><span data-stu-id="aca44-146">In some scenarios, you may want to run Windows PowerShell scripts on the computer that builds your projects.</span></span> <span data-ttu-id="aca44-147">Ad esempio, si potrebbe usare uno script di Windows PowerShell per pulire le cartelle di compilazione o la scrittura di voci in un file di log personalizzato.</span><span class="sxs-lookup"><span data-stu-id="aca44-147">For example, you might use a Windows PowerShell script to clean up build folders or write entries to a custom log file.</span></span>

<span data-ttu-id="aca44-148">In termini di sintassi, esecuzione di uno script di Windows PowerShell da un file di progetto MSBuild è equivale all'esecuzione di uno script di Windows PowerShell da un prompt dei comandi normale.</span><span class="sxs-lookup"><span data-stu-id="aca44-148">In terms of syntax, running a Windows PowerShell script from an MSBuild project file is the same as running a Windows PowerShell script from a regular command prompt.</span></span> <span data-ttu-id="aca44-149">È necessario richiamare il powershell.exe eseguibile e usare la **– comando** commutatore per fornire i comandi che si desidera che Windows PowerShell per l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="aca44-149">You need to invoke the powershell.exe executable and use the **–command** switch to provide the commands you want Windows PowerShell to run.</span></span> <span data-ttu-id="aca44-150">(In Windows PowerShell v2, è anche possibile usare la **– file** switch).</span><span class="sxs-lookup"><span data-stu-id="aca44-150">(In Windows PowerShell v2, you can also use the **–file** switch).</span></span> <span data-ttu-id="aca44-151">Il comando deve accettare questo formato:</span><span class="sxs-lookup"><span data-stu-id="aca44-151">The command should take this format:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


<span data-ttu-id="aca44-152">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="aca44-152">For example:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


<span data-ttu-id="aca44-153">Se il percorso per lo script include spazi, è necessario racchiudere il percorso del file tra virgolette singole preceduto da una e commerciale.</span><span class="sxs-lookup"><span data-stu-id="aca44-153">If the path to your script includes spaces, you need to enclose the file path in single quotes preceded by an ampersand.</span></span> <span data-ttu-id="aca44-154">È possibile usare le virgolette doppie, perché è già stata usata per racchiudere il comando:</span><span class="sxs-lookup"><span data-stu-id="aca44-154">You can't use double quotes, because you've already used them to enclose the command:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


<span data-ttu-id="aca44-155">Esistono alcune considerazioni aggiuntive quando si richiama questo comando da MSBuild.</span><span class="sxs-lookup"><span data-stu-id="aca44-155">There are a few additional considerations when you invoke this command from MSBuild.</span></span> <span data-ttu-id="aca44-156">In primo luogo, è necessario includere il **– NonInteractive** flag per verificare che lo script viene eseguito in modalità non interattiva.</span><span class="sxs-lookup"><span data-stu-id="aca44-156">First, you should include the **–NonInteractive** flag to ensure that the script executes quietly.</span></span> <span data-ttu-id="aca44-157">Successivamente, è necessario includere il **– ExecutionPolicy** flag con valore dell'argomento appropriato.</span><span class="sxs-lookup"><span data-stu-id="aca44-157">Next, you should include the **–ExecutionPolicy** flag with an appropriate argument value.</span></span> <span data-ttu-id="aca44-158">Specifica i criteri di esecuzione che Windows PowerShell verranno applicate per lo script e consente di ignorare i criteri di esecuzione predefinito, che potrebbero impedire l'esecuzione dello script.</span><span class="sxs-lookup"><span data-stu-id="aca44-158">This specifies the execution policy that Windows PowerShell will apply to your script and allows you to override the default execution policy, which may prevent execution of your script.</span></span> <span data-ttu-id="aca44-159">È possibile scegliere tra i valori di argomento:</span><span class="sxs-lookup"><span data-stu-id="aca44-159">You can choose from these argument values:</span></span>

- <span data-ttu-id="aca44-160">Un valore pari **Unrestricted** consentirà di Windows PowerShell eseguire lo script, indipendentemente dal fatto che lo script è firmato.</span><span class="sxs-lookup"><span data-stu-id="aca44-160">A value of **Unrestricted** will allow Windows PowerShell to execute your script, regardless of whether the script is signed.</span></span>
- <span data-ttu-id="aca44-161">Un valore pari **RemoteSigned** consentirà di Windows PowerShell eseguire gli script non firmati creati nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="aca44-161">A value of **RemoteSigned** will allow Windows PowerShell to execute unsigned scripts that were created on the local machine.</span></span> <span data-ttu-id="aca44-162">Tuttavia, gli script creati in un' posizione devono essere firmati.</span><span class="sxs-lookup"><span data-stu-id="aca44-162">However, scripts that were created elsewhere must be signed.</span></span> <span data-ttu-id="aca44-163">(In pratica, è molto improbabile che hanno creato uno script di Windows PowerShell in locale in un server di compilazione).</span><span class="sxs-lookup"><span data-stu-id="aca44-163">(In practice, you're very unlikely to have created a Windows PowerShell script locally on a build server).</span></span>
- <span data-ttu-id="aca44-164">Un valore pari **AllSigned** consentirà di Windows PowerShell eseguire solo script firmati.</span><span class="sxs-lookup"><span data-stu-id="aca44-164">A value of **AllSigned** will allow Windows PowerShell to execute signed scripts only.</span></span>

<span data-ttu-id="aca44-165">I criteri di esecuzione predefinito sono **Restricted**, che impedisce l'esecuzione di qualsiasi file di script di Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="aca44-165">The default execution policy is **Restricted**, which prevents Windows PowerShell from running any script files.</span></span>

<span data-ttu-id="aca44-166">Infine, è necessario eseguire l'escape di tutti i caratteri XML riservati che si verificano nel comando di Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="aca44-166">Finally, you need to escape any reserved XML characters that occur in your Windows PowerShell command:</span></span>

- <span data-ttu-id="aca44-167">Sostituire le virgolette singole con  **&amp;apos;**</span><span class="sxs-lookup"><span data-stu-id="aca44-167">Replace single quotes with **&amp;apos;**</span></span>
- <span data-ttu-id="aca44-168">Sostituire le virgolette doppie con  **&amp;quot;**</span><span class="sxs-lookup"><span data-stu-id="aca44-168">Replace double quotes with **&amp;quot;**</span></span>
- <span data-ttu-id="aca44-169">Sostituire le e commerciali con  **&amp;amp;**</span><span class="sxs-lookup"><span data-stu-id="aca44-169">Replace ampersands with **&amp;amp;**</span></span>

- <span data-ttu-id="aca44-170">Quando si apportano queste modifiche, il comando sarà analogo al seguente:</span><span class="sxs-lookup"><span data-stu-id="aca44-170">When you make these changes, your command will resemble this:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


<span data-ttu-id="aca44-171">All'interno del file di progetto MSBuild personalizzato, è possibile creare una nuova destinazione e usare la **Exec** attività per eseguire questo comando:</span><span class="sxs-lookup"><span data-stu-id="aca44-171">Within your custom MSBuild project file, you can create a new target and use the **Exec** task to run this command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


<span data-ttu-id="aca44-172">In questo esempio, si noti che:</span><span class="sxs-lookup"><span data-stu-id="aca44-172">In this example, note that:</span></span>

- <span data-ttu-id="aca44-173">Tutte le variabili, ad esempio i valori dei parametri e il percorso dell'eseguibile di Windows PowerShell, vengono dichiarate come proprietà di MSBuild.</span><span class="sxs-lookup"><span data-stu-id="aca44-173">Any variables, like parameter values and the location of the Windows PowerShell executable, are declared as MSBuild properties.</span></span>
- <span data-ttu-id="aca44-174">Sono incluse le condizioni per consentire agli utenti di eseguire l'override di questi valori dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="aca44-174">Conditions are included to enable users to override these values from the command line.</span></span>
- <span data-ttu-id="aca44-175">Il **MSDeployComputerName** proprietà è dichiarata in un' posizione nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="aca44-175">The **MSDeployComputerName** property is declared elsewhere in the project file.</span></span>

<span data-ttu-id="aca44-176">Quando si esegue questa destinazione come parte del processo di compilazione, Windows PowerShell verrà eseguito il comando e scrivere una voce di log per il file specificato.</span><span class="sxs-lookup"><span data-stu-id="aca44-176">When you execute this target as part of your build process, Windows PowerShell will run your command and write a log entry to the file you specified.</span></span>

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a><span data-ttu-id="aca44-177">L'esecuzione di uno Script di Windows PowerShell in un Computer remoto</span><span class="sxs-lookup"><span data-stu-id="aca44-177">Executing a Windows PowerShell Script on a Remote Computer</span></span>

<span data-ttu-id="aca44-178">Windows PowerShell è in grado di eseguire gli script nei computer remoti tramite [gestione remota Windows](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span><span class="sxs-lookup"><span data-stu-id="aca44-178">Windows PowerShell is capable of running scripts on remote computers through [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span></span> <span data-ttu-id="aca44-179">A tale scopo, è necessario usare il [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="aca44-179">To do this, you need to use the [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet.</span></span> <span data-ttu-id="aca44-180">Ciò consente di eseguire lo script su una o più computer remoti senza copiare lo script per i computer remoti.</span><span class="sxs-lookup"><span data-stu-id="aca44-180">This lets you execute your script against one or more remote computers without copying the script to the remote computers.</span></span> <span data-ttu-id="aca44-181">I risultati vengono restituiti al computer locale da cui è stato eseguito lo script.</span><span class="sxs-lookup"><span data-stu-id="aca44-181">Any results are returned to the local computer from which you ran the script.</span></span>

> [!NOTE]
> <span data-ttu-id="aca44-182">Prima di usare la **Invoke-Command** generare script per cmdlet per eseguire Windows PowerShell in un computer remoto, è necessario configurare un listener WinRM per accettare i messaggi remoti.</span><span class="sxs-lookup"><span data-stu-id="aca44-182">Before you use the **Invoke-Command** cmdlet to execute Windows PowerShell scripts on a remote computer, you need to configure a WinRM listener to accept remote messages.</span></span> <span data-ttu-id="aca44-183">È possibile farlo eseguendo il comando **winrm quickconfig** nel computer remoto.</span><span class="sxs-lookup"><span data-stu-id="aca44-183">You can do this by running the command **winrm quickconfig** on the remote computer.</span></span> <span data-ttu-id="aca44-184">Per altre informazioni, vedere [installazione e configurazione di Windows gestione remota](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="aca44-184">For more information, see [Installation and Configuration for Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span></span>


<span data-ttu-id="aca44-185">Da una finestra di Windows PowerShell, è necessario utilizzare questa sintassi per eseguire la **LogDeploy.ps1** script in un computer remoto:</span><span class="sxs-lookup"><span data-stu-id="aca44-185">From a Windows PowerShell window, you'd use this syntax to run the **LogDeploy.ps1** script on a remote computer:</span></span>


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> <span data-ttu-id="aca44-186">Esistono diversi altri modi di utilizzo **Invoke-Command** per eseguire uno script di file, ma questo approccio è la più semplice quando è necessario fornire i valori dei parametri e gestire i percorsi contenenti spazi.</span><span class="sxs-lookup"><span data-stu-id="aca44-186">There are various other ways of using **Invoke-Command** to run a script file, but this approach is the most straightforward when you need to provide parameter values and manage paths with spaces.</span></span>


<span data-ttu-id="aca44-187">Quando si esegue questo prompt dei comandi, è necessario richiamare il comando eseguibile di Windows PowerShell e usare la **– comando** parametro per specificare le istruzioni:</span><span class="sxs-lookup"><span data-stu-id="aca44-187">When you run this from a command prompt, you need to invoke the Windows PowerShell executable and use the **–command** parameter to provide your instructions:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


<span data-ttu-id="aca44-188">Come prima, è necessario fornire alcune opzioni aggiuntive per evitare l'utilizzo di tutti i caratteri XML riservati quando si esegue il comando da MSBuild:</span><span class="sxs-lookup"><span data-stu-id="aca44-188">As before, you need to provide some additional switches and escape any reserved XML characters when you run the command from MSBuild:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


<span data-ttu-id="aca44-189">Infine, come in precedenza, è possibile usare la **Exec** attività all'interno di una destinazione di MSBuild personalizzata per eseguire il comando:</span><span class="sxs-lookup"><span data-stu-id="aca44-189">Finally, as before, you can use the **Exec** task within a custom MSBuild target to execute your command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


<span data-ttu-id="aca44-190">Quando si esegue questa destinazione come parte del processo di compilazione, Windows PowerShell verrà eseguito lo script nel computer specificato nel **– computername** argomento.</span><span class="sxs-lookup"><span data-stu-id="aca44-190">When you execute this target as part of your build process, Windows PowerShell will run your script on the computer you specified in the **–computername** argument.</span></span>

## <a name="conclusion"></a><span data-ttu-id="aca44-191">Conclusione</span><span class="sxs-lookup"><span data-stu-id="aca44-191">Conclusion</span></span>

<span data-ttu-id="aca44-192">In questo argomento viene descritto come eseguire uno script di Windows PowerShell da un file di progetto MSBuild.</span><span class="sxs-lookup"><span data-stu-id="aca44-192">This topic described how to run a Windows PowerShell script from an MSBuild project file.</span></span> <span data-ttu-id="aca44-193">È possibile usare questo approccio per eseguire uno script di Windows PowerShell in locale o in un computer remoto, come parte di un processo di compilazione e distribuzione automatizzato o passo a passo.</span><span class="sxs-lookup"><span data-stu-id="aca44-193">You can use this approach to run a Windows PowerShell script, either locally or on a remote computer, as part of an automated or single-step build and deployment process.</span></span>

## <a name="further-reading"></a><span data-ttu-id="aca44-194">Ulteriori informazioni</span><span class="sxs-lookup"><span data-stu-id="aca44-194">Further Reading</span></span>

<span data-ttu-id="aca44-195">Per indicazioni sulla firma degli script di Windows PowerShell e la gestione dei criteri di esecuzione, vedere [esecuzione di script di Windows PowerShell](https://technet.microsoft.com/library/ee176949.aspx).</span><span class="sxs-lookup"><span data-stu-id="aca44-195">For guidance on signing Windows PowerShell scripts and managing execution policies, see [Running Windows PowerShell Scripts](https://technet.microsoft.com/library/ee176949.aspx).</span></span> <span data-ttu-id="aca44-196">Per informazioni dettagliate sull'esecuzione di comandi di Windows PowerShell da un computer remoto, vedere [esecuzione di comandi remoti](https://technet.microsoft.com/library/dd819505.aspx).</span><span class="sxs-lookup"><span data-stu-id="aca44-196">For guidance on running Windows PowerShell commands from a remote computer, see [Running Remote Commands](https://technet.microsoft.com/library/dd819505.aspx).</span></span>

<span data-ttu-id="aca44-197">Per altre informazioni sull'utilizzo dei file di progetto MSBuild personalizzati per controllare il processo di distribuzione, vedere [informazioni sul File di progetto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [informazioni sul processo di compilazione](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="aca44-197">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="aca44-198">[Precedente](taking-web-applications-offline-with-web-deploy.md)
> [Successivo](troubleshooting-the-packaging-process.md)</span><span class="sxs-lookup"><span data-stu-id="aca44-198">[Previous](taking-web-applications-offline-with-web-deploy.md)
[Next](troubleshooting-the-packaging-process.md)</span></span>
