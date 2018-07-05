---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Creazione ed esecuzione di una distribuzione di File di comando | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come creare un file di comando che consentirà di eseguire una distribuzione usando i file di progetto di Microsoft Build Engine (MSBuild) come un unico passaggio, re...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: 6b2a75e0740f648a3a6e4f39c00bd30609325728
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830302"
---
<a name="creating-and-running-a-deployment-command-file"></a><span data-ttu-id="9b7bf-103">Creazione ed esecuzione di un File di comando di distribuzione</span><span class="sxs-lookup"><span data-stu-id="9b7bf-103">Creating and Running a Deployment Command File</span></span>
====================
<span data-ttu-id="9b7bf-104">da [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="9b7bf-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="9b7bf-105">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="9b7bf-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="9b7bf-106">Questo argomento descrive come creare un file di comando che consentirà di eseguire una distribuzione con i file di progetto di Microsoft Build Engine (MSBuild) con un processo ripetibile, passo a passo.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-106">This topic describes how to build a command file that will let you run a deployment using Microsoft Build Engine (MSBuild) project files as a single-step, repeatable process.</span></span>


<span data-ttu-id="9b7bf-107">In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione aziendale di una società fittizia, denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [Contact Manager](the-contact-manager-solution.md) soluzione&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="9b7bf-108">Il metodo di distribuzione il fulcro delle esercitazioni seguenti si basa sull'approccio di file di progetto split descritto in [informazioni sul processo di compilazione](understanding-the-build-process.md), in cui il processo di compilazione è controllato da due file di progetto&#x2014;contenente una istruzioni che si applicano a tutti gli ambienti di destinazione e quella che contiene le impostazioni di compilazione e distribuzione specifiche dell'ambiente di creazione.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="9b7bf-109">In fase di compilazione, il file di progetto specifici dell'ambiente viene unito nel file di progetto indipendenti dall'ambiente in modo da formare un set completo di istruzioni di compilazione.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="process-overview"></a><span data-ttu-id="9b7bf-110">Panoramica del processo</span><span class="sxs-lookup"><span data-stu-id="9b7bf-110">Process Overview</span></span>

<span data-ttu-id="9b7bf-111">In questo argomento si apprenderà come creare ed eseguire un file di comando che usa questi file di progetto per eseguire una distribuzione ripetibile per l'ambiente di destinazione.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-111">In this topic, you'll learn how to create and run a command file that uses these project files to perform a repeatable deployment to your target environment.</span></span> <span data-ttu-id="9b7bf-112">In pratica, il file di comando semplicemente deve contenere un comando di MSBuild che:</span><span class="sxs-lookup"><span data-stu-id="9b7bf-112">Essentially, the command file simply needs to contain an MSBuild command that:</span></span>

- <span data-ttu-id="9b7bf-113">Indica a MSBuild per eseguire l'ambiente indipendente *Publish.proj* file.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-113">Tells MSBuild to execute the environment-agnostic *Publish.proj* file.</span></span>
- <span data-ttu-id="9b7bf-114">Indica la *Publish.proj* file quale file contiene le impostazioni di progetto specifici dell'ambiente e dove reperirla.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-114">Tells the *Publish.proj* file which file contains the environment-specific project settings and where to find it.</span></span>

## <a name="create-an-msbuild-command"></a><span data-ttu-id="9b7bf-115">Creare un comando di MSBuild</span><span class="sxs-lookup"><span data-stu-id="9b7bf-115">Create an MSBuild Command</span></span>

<span data-ttu-id="9b7bf-116">Come descritto in [informazioni sul processo di compilazione](understanding-the-build-process.md), il file di progetto specifici dell'ambiente&#x2014;, ad esempio, *Env-Dev.proj*&#x2014;è progettato per essere importato in ambiente indipendente *Publish.proj* file in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-116">As described in [Understanding the Build Process](understanding-the-build-process.md), the environment-specific project file&#x2014;for example, *Env-Dev.proj*&#x2014;is designed to be imported into the environment-agnostic *Publish.proj* file at build time.</span></span> <span data-ttu-id="9b7bf-117">Insieme, questi due file forniscono un set completo di istruzioni che indicano come compilare e distribuire la soluzione di MSBuild.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-117">Together, these two files provide a complete set of instructions that tell MSBuild how to build and deploy your solution.</span></span>

<span data-ttu-id="9b7bf-118">Il *Publish.proj* file usa una **importare** elemento per importare il file di progetto specifici dell'ambiente.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-118">The *Publish.proj* file uses an **Import** element to import the environment-specific project file.</span></span>


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


<span data-ttu-id="9b7bf-119">Di conseguenza, quando si usa MSBuild.exe per compilare e distribuire la soluzione Contact Manager, è necessario:</span><span class="sxs-lookup"><span data-stu-id="9b7bf-119">As such, when you use MSBuild.exe to build and deploy the Contact Manager solution, you need to:</span></span>

- <span data-ttu-id="9b7bf-120">Eseguire MSBuild.exe nel *Publish.proj* file.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-120">Run MSBuild.exe on the *Publish.proj* file.</span></span>
- <span data-ttu-id="9b7bf-121">Specificare il percorso del file di progetto specifici dell'ambiente, fornendo un parametro della riga di comando denominato **TargetEnvPropsFile**.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-121">Specify the location of the environment-specific project file by supplying a command-line parameter named **TargetEnvPropsFile**.</span></span>

<span data-ttu-id="9b7bf-122">A tale scopo, il comando di MSBuild sarà analogo al seguente:</span><span class="sxs-lookup"><span data-stu-id="9b7bf-122">To do this, your MSBuild command should resemble this:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


<span data-ttu-id="9b7bf-123">A questo punto, è un semplice passaggio per spostare in una distribuzione ripetibile, passo a passo.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-123">From here, it's a simple step to move to a repeatable, single-step deployment.</span></span> <span data-ttu-id="9b7bf-124">È necessario eseguire è sufficiente aggiungere il comando di MSBuild in un file con estensione cmd.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-124">All you need to do is to add your MSBuild command to a .cmd file.</span></span> <span data-ttu-id="9b7bf-125">Nella soluzione Contact Manager, la cartella di pubblicazione include un file denominato *Publish-Dev.cmd* che esegue esattamente questa.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-125">In the Contact Manager solution, the Publish folder includes a file named *Publish-Dev.cmd* that does exactly this.</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="9b7bf-126">Il **/fl** indica a MSBuild per creare un file di log denominato *MSBuild* nella directory di lavoro in cui è stata richiamata MSBuild.exe.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-126">The **/fl** switch instructs MSBuild to create a log file named *msbuild.log* in the working directory in which MSBuild.exe was invoked.</span></span>


<span data-ttu-id="9b7bf-127">Per distribuire o ridistribuire la soluzione Contact Manager, è sufficiente per eseguire operazioni viene eseguito il *Publish-Dev.cmd* file.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-127">To deploy or redeploy the Contact Manager solution, all you need to do is run the *Publish-Dev.cmd* file.</span></span> <span data-ttu-id="9b7bf-128">Quando si esegue il file, MSBuild sarà:</span><span class="sxs-lookup"><span data-stu-id="9b7bf-128">When you run the file, MSBuild will:</span></span>

- <span data-ttu-id="9b7bf-129">Compilare tutti i progetti nella soluzione.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-129">Build all the projects in the solution.</span></span>
- <span data-ttu-id="9b7bf-130">Generazione di pacchetti web distribuibili per i progetti di applicazione web.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-130">Generate deployable web packages for the web application projects.</span></span>
- <span data-ttu-id="9b7bf-131">Generare file dbschema e deploymanifest per i progetti di database.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-131">Generate .dbschema and .deploymanifest files for the database projects.</span></span>
- <span data-ttu-id="9b7bf-132">Distribuire i pacchetti web al server web.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-132">Deploy the web packages to the web server.</span></span>
- <span data-ttu-id="9b7bf-133">Distribuire il database al server di database.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-133">Deploy the database to the database server.</span></span>

## <a name="run-the-deployment"></a><span data-ttu-id="9b7bf-134">Eseguire la distribuzione</span><span class="sxs-lookup"><span data-stu-id="9b7bf-134">Run the Deployment</span></span>

<span data-ttu-id="9b7bf-135">Dopo aver creato un file di comando per l'ambiente di destinazione, sarà possibile completare l'intera distribuzione eseguendo semplicemente il file.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-135">When you've created a command file for your target environment, you should be able to complete the entire deployment by simply running the file.</span></span>

<span data-ttu-id="9b7bf-136">**Per distribuire la soluzione Contact Manager nell'ambiente di test**</span><span class="sxs-lookup"><span data-stu-id="9b7bf-136">**To deploy the Contact Manager solution to your test environment**</span></span>

1. <span data-ttu-id="9b7bf-137">Nella workstation per gli sviluppatori, aprire Windows Explorer e quindi passare al percorso dei *Publish-Dev.cmd* file.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-137">On your developer workstation, open Windows Explorer, and then browse to the location of the *Publish-Dev.cmd* file.</span></span>
2. <span data-ttu-id="9b7bf-138">Fare doppio clic sul file per l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-138">Double-click the file to run it.</span></span>
3. <span data-ttu-id="9b7bf-139">Se un' **Apri File – avviso di sicurezza** verrà visualizzata la finestra di dialogo, fare clic su **eseguire**.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-139">If an **Open File – Security Warning** dialog box appears, click **Run**.</span></span>
4. <span data-ttu-id="9b7bf-140">Se le impostazioni di configurazione e il server di test sono configurati correttamente, la finestra del prompt dei comandi visualizzerà un **compilazione ha avuto esito positivo** dei messaggi quando MSBuild ha terminato l'elaborazione di file di progetto.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-140">If your configuration settings and test servers are set up correctly, the Command Prompt window will show a **Build succeeded** message when MSBuild has finished processing the project files.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. <span data-ttu-id="9b7bf-141">Se questa è la prima volta che è stata distribuita la soluzione in questo ambiente, è necessario aggiungere l'account del computer server web di test per il **db\_datawriter** e **db\_datareader**ruoli su di **ContactManager** database.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-141">If this is the first time you've deployed the solution to this environment, you'll need to add the test web server machine account to the **db\_datawriter** and **db\_datareader** roles on the **ContactManager** database.</span></span> <span data-ttu-id="9b7bf-142">Questa procedura è descritta in [configurare un Server di Database per la pubblicazione di distribuzione Web](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="9b7bf-142">This procedure is described in [Configure a Database Server for Web Deploy Publishing](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="9b7bf-143">Devi solo assegnare queste autorizzazioni quando si crea il database.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-143">You only need to assign these permissions when you create the database.</span></span> <span data-ttu-id="9b7bf-144">Per impostazione predefinita, il processo di compilazione non verrà ricreato il database in ogni distribuzione&#x2014;, invece verrà confrontare il database esistente per lo schema più recente e assegnare solo le modifiche necessarie.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-144">By default, the build process will not recreate the database on every deployment&#x2014;instead, it will compare the existing database to the latest schema and make only the changes required.</span></span> <span data-ttu-id="9b7bf-145">Di conseguenza, è solo necessario eseguire il mapping di questi ruoli del database alla prima che distribuzione della soluzione.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-145">As a result, you should only need to map these database roles the first time you deploy the solution.</span></span>
6. <span data-ttu-id="9b7bf-146">Aprire Internet Explorer e passare all'URL dell'applicazione Contact Manager (ad esempio, `http://testweb1:85/ContactManager/`).</span><span class="sxs-lookup"><span data-stu-id="9b7bf-146">Open Internet Explorer and browse to the URL of the Contact Manager application (for example, `http://testweb1:85/ContactManager/`).</span></span>
7. <span data-ttu-id="9b7bf-147">Verificare che l'applicazione funzioni come previsto e che sia possibile aggiungere contatti.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-147">Verify that the application works as expected and you're able to add contacts.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="9b7bf-148">Conclusione</span><span class="sxs-lookup"><span data-stu-id="9b7bf-148">Conclusion</span></span>

<span data-ttu-id="9b7bf-149">Creazione di un file di comando che contiene le istruzioni di MSBuild offre un modo facile e veloce di creazione e distribuzione di una soluzione multiprogetto in un ambiente di destinazione specifico.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-149">Creating a command file containing your MSBuild instructions provides you with a quick and easy way of building and deploying a multi-project solution to a specific destination environment.</span></span> <span data-ttu-id="9b7bf-150">Se si desidera distribuire ripetutamente la soluzione in più ambienti di destinazione, è possibile creare più file di comando.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-150">If you need to repeatedly deploy your solution to multiple destination environments, you can create multiple command files.</span></span> <span data-ttu-id="9b7bf-151">In ogni file di comando, il comando di MSBuild compilerà il file di progetto universale stesso, ma viene specificato un file di progetto specifici dell'ambiente diverso.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-151">In each command file, the MSBuild command will build the same universal project file, but it will specify a different environment-specific project file.</span></span> <span data-ttu-id="9b7bf-152">Ad esempio, un file di comando per la pubblicazione a uno sviluppatore o ambiente di test potrebbe contenere questo comando di MSBuild:</span><span class="sxs-lookup"><span data-stu-id="9b7bf-152">For example, a command file to publish to a developer or test environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


<span data-ttu-id="9b7bf-153">Un file di comando per la pubblicazione in un ambiente di staging potrebbe contenere questo comando di MSBuild:</span><span class="sxs-lookup"><span data-stu-id="9b7bf-153">A command file to publish to a staging environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> <span data-ttu-id="9b7bf-154">Per indicazioni su come personalizzare i file di progetto specifici dell'ambiente per ambienti server, vedere [configurare le proprietà di distribuzione per un ambiente di destinazione](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span><span class="sxs-lookup"><span data-stu-id="9b7bf-154">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


<span data-ttu-id="9b7bf-155">È anche possibile personalizzare il processo di compilazione per ogni ambiente mediante l'override delle proprietà o l'impostazione di varie altre opzioni nel comando di MSBuild.</span><span class="sxs-lookup"><span data-stu-id="9b7bf-155">You can also customize the build process for each environment by overriding properties or setting various other switches in your MSBuild command.</span></span> <span data-ttu-id="9b7bf-156">Per altre informazioni, vedere [riferimenti alla riga di comando di MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).</span><span class="sxs-lookup"><span data-stu-id="9b7bf-156">For more information, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9b7bf-157">[Precedente](deploying-database-projects.md)
> [Successivo](manually-installing-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="9b7bf-157">[Previous](deploying-database-projects.md)
[Next](manually-installing-web-packages.md)</span></span>
