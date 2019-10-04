---
title: Pubblicare un'app ASP.NET Core in IIS
author: guardrex
description: Informazioni su come ospitare un'app ASP.NET Core in un server IIS.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/03/2019
uid: tutorials/publish-to-iis
ms.openlocfilehash: 820527cc15f883c906d2fdf1c073d443a5b3b40e
ms.sourcegitcommit: d8b12cc1716ee329d7bd2300e201b61e15d506ac
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2019
ms.locfileid: "71942876"
---
# <a name="publish-an-aspnet-core-app-to-iis"></a><span data-ttu-id="ff765-103">Pubblicare un'app ASP.NET Core in IIS</span><span class="sxs-lookup"><span data-stu-id="ff765-103">Publish an ASP.NET Core app to IIS</span></span>

<span data-ttu-id="ff765-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ff765-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ff765-105">Questa esercitazione mostra come ospitare un'app ASP.NET Core in un server IIS.</span><span class="sxs-lookup"><span data-stu-id="ff765-105">This tutorial shows how to host an ASP.NET Core app on an IIS server.</span></span>

<span data-ttu-id="ff765-106">Questa esercitazione illustra le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="ff765-106">This tutorial covers the following subjects:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ff765-107">Installare il bundle di hosting di.NET Core in Windows Server.</span><span class="sxs-lookup"><span data-stu-id="ff765-107">Install the .NET Core Hosting Bundle on Windows Server.</span></span>
> * <span data-ttu-id="ff765-108">Creare un sito IIS in Gestione IIS.</span><span class="sxs-lookup"><span data-stu-id="ff765-108">Create an IIS site in IIS Manager.</span></span>
> * <span data-ttu-id="ff765-109">Distribuire un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ff765-109">Deploy an ASP.NET Core app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ff765-110">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="ff765-110">Prerequisites</span></span>

* <span data-ttu-id="ff765-111">[.NET Core SDK](/dotnet/core/sdk) installato nel computer di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="ff765-111">[.NET Core SDK](/dotnet/core/sdk) installed on the development machine.</span></span>
* <span data-ttu-id="ff765-112">Windows Server configurato con il ruolo del server **Server Web (IIS)** .</span><span class="sxs-lookup"><span data-stu-id="ff765-112">Windows Server configured with the **Web Server (IIS)** server role.</span></span> <span data-ttu-id="ff765-113">Se il server non è configurato per ospitare siti Web con IIS, seguire le istruzioni nella sezione *Configurazione di IIS* dell'articolo <xref:host-and-deploy/iis/index#iis-configuration> e quindi tornare a questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="ff765-113">If your server isn't configured to host websites with IIS, follow the guidance in the *IIS configuration* section of the <xref:host-and-deploy/iis/index#iis-configuration> article and then return to this tutorial.</span></span>

> [!WARNING]
> <span data-ttu-id="ff765-114">**La configurazione di IIS e la sicurezza del sito Web coinvolgono concetti non trattati in questa esercitazione.**</span><span class="sxs-lookup"><span data-stu-id="ff765-114">**IIS configuration and website security involve concepts that aren't covered by this tutorial.**</span></span> <span data-ttu-id="ff765-115">Consultare le linee guida per IIS nella [documentazione di Microsoft IIS](https://www.iis.net/) e l'[articolo di ASP.NET Core sull'hosting con IIS](xref:host-and-deploy/iis/index) prima di ospitare app di produzione in IIS.</span><span class="sxs-lookup"><span data-stu-id="ff765-115">Consult the IIS guidance in the [Microsoft IIS documentation](https://www.iis.net/) and the [ASP.NET Core article on hosting with IIS](xref:host-and-deploy/iis/index) before hosting production apps on IIS.</span></span>
>
> <span data-ttu-id="ff765-116">Gli scenari importanti per l'hosting di IIS non trattati in questa esercitazione includono:</span><span class="sxs-lookup"><span data-stu-id="ff765-116">Important scenarios for IIS hosting not covered by this tutorial include:</span></span>
>
> * [<span data-ttu-id="ff765-117">Creazione di un hive del Registro di sistema per la protezione dei dati ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ff765-117">Creation of a registry hive for ASP.NET Core Data Protection</span></span>](xref:host-and-deploy/iis/index#data-protection)
> * [<span data-ttu-id="ff765-118">Configurazione dell'elenco di controllo di accesso (ACL) del pool di app</span><span class="sxs-lookup"><span data-stu-id="ff765-118">Configuration of the app pool's Access Control List (ACL)</span></span>](xref:host-and-deploy/iis/index#application-pool-identity)
> * <span data-ttu-id="ff765-119">Per concentrarsi sui concetti correlati alla distribuzione di IIS, questa esercitazione illustra come distribuire un'app senza sicurezza HTTPS configurata in IIS.</span><span class="sxs-lookup"><span data-stu-id="ff765-119">To focus on IIS deployment concepts, this tutorial deploys an app without HTTPS security configured in IIS.</span></span> <span data-ttu-id="ff765-120">Per altre informazioni sull'hosting di un'app abilitata per il protocollo HTTPS, vedere gli argomenti relativi alla sicurezza nella sezione [Risorse aggiuntive](#additional-resources) di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="ff765-120">For more information on hosting an app enabled for HTTPS protocol, see the security topics in the [Additional resources](#additional-resources) section of this article.</span></span> <span data-ttu-id="ff765-121">Altre indicazioni per l'hosting di app ASP.NET Core sono disponibili nell'articolo <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="ff765-121">Further guidance for hosting ASP.NET Core apps is provided in the <xref:host-and-deploy/iis/index> article.</span></span>

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="ff765-122">Installare il bundle di hosting .NET Core</span><span class="sxs-lookup"><span data-stu-id="ff765-122">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="ff765-123">Installare il *bundle di hosting .NET Core* nel server IIS.</span><span class="sxs-lookup"><span data-stu-id="ff765-123">Install the *.NET Core Hosting Bundle* on the IIS server.</span></span> <span data-ttu-id="ff765-124">L'aggregazione installa il runtime di .NET Core, la libreria di .NET Core e il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="ff765-124">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="ff765-125">Il modulo consente l'esecuzione delle app ASP.NET Core dietro IIS.</span><span class="sxs-lookup"><span data-stu-id="ff765-125">The module allows ASP.NET Core apps to run behind IIS.</span></span>

<span data-ttu-id="ff765-126">Scaricare il programma di installazione mediante il collegamento seguente:</span><span class="sxs-lookup"><span data-stu-id="ff765-126">Download the installer using the following link:</span></span>

[<span data-ttu-id="ff765-127">Programma di installazione del bundle di hosting .NET Core corrente (download diretto)</span><span class="sxs-lookup"><span data-stu-id="ff765-127">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

1. <span data-ttu-id="ff765-128">Eseguire il programma di installazione nel server IIS.</span><span class="sxs-lookup"><span data-stu-id="ff765-128">Run the installer on the IIS server.</span></span>

1. <span data-ttu-id="ff765-129">Riavviare il server o eseguire **net stop was /y** seguito da **net start w3svc** in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="ff765-129">Restart the server or execute **net stop was /y** followed by **net start w3svc** in a command shell.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="ff765-130">Creare il sito IIS</span><span class="sxs-lookup"><span data-stu-id="ff765-130">Create the IIS site</span></span>

1. <span data-ttu-id="ff765-131">Nel server IIS creare una cartella per contenere le cartelle e i file pubblicati dell'app.</span><span class="sxs-lookup"><span data-stu-id="ff765-131">On the IIS server, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="ff765-132">In un passaggio successivo, il percorso della cartella viene fornito a IIS come percorso fisico dell'app.</span><span class="sxs-lookup"><span data-stu-id="ff765-132">In a following step, the folder's path is provided to IIS as the physical path to the app.</span></span>

1. <span data-ttu-id="ff765-133">In Gestione IIS aprire il nodo del server nel pannello **Connessioni**.</span><span class="sxs-lookup"><span data-stu-id="ff765-133">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="ff765-134">Fare clic con il pulsante destro del mouse sulla cartella **Siti**.</span><span class="sxs-lookup"><span data-stu-id="ff765-134">Right-click the **Sites** folder.</span></span> <span data-ttu-id="ff765-135">Scegliere **Aggiungi sito Web** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="ff765-135">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="ff765-136">Specificare un **Nome del sito** e impostare il **Percorso fisico** per la cartella di distribuzione dell'app che è stata creata.</span><span class="sxs-lookup"><span data-stu-id="ff765-136">Provide a **Site name** and set the **Physical path** to the app's deployment folder that you created.</span></span> <span data-ttu-id="ff765-137">Specificare la configurazione in **Binding** e creare il sito Web selezionando **OK**.</span><span class="sxs-lookup"><span data-stu-id="ff765-137">Provide the **Binding** configuration and create the website by selecting **OK**.</span></span>

## <a name="create-an-aspnet-core-razor-pages-app"></a><span data-ttu-id="ff765-138">Creare un'app ASP.NET Core Razor Pages</span><span class="sxs-lookup"><span data-stu-id="ff765-138">Create an ASP.NET Core Razor Pages app</span></span>

<span data-ttu-id="ff765-139">Seguire l'esercitazione <xref:getting-started> per creare un'app Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="ff765-139">Follow the <xref:getting-started> tutorial to create a Razor Pages app.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="ff765-140">Pubblicare e distribuire l'app</span><span class="sxs-lookup"><span data-stu-id="ff765-140">Publish and deploy the app</span></span>

<span data-ttu-id="ff765-141">*Pubblicare un'app* significa creare un'app compilata che può essere ospitata da un server.</span><span class="sxs-lookup"><span data-stu-id="ff765-141">*Publish an app* means to produce a compiled app that can be hosted by a server.</span></span> <span data-ttu-id="ff765-142">*Distribuire un'app* significa spostare l'app pubblicata in un sistema di hosting.</span><span class="sxs-lookup"><span data-stu-id="ff765-142">*Deploy an app* means to move the published app to a hosting system.</span></span> <span data-ttu-id="ff765-143">Il passaggio di pubblicazione viene gestito da [.NET Core SDK](/dotnet/core/sdk), mentre la fase di distribuzione può essere gestita tramite vari approcci.</span><span class="sxs-lookup"><span data-stu-id="ff765-143">The publish step is handled by the [.NET Core SDK](/dotnet/core/sdk), while the deployment step can be handled by a variety of approaches.</span></span> <span data-ttu-id="ff765-144">Questa esercitazione adotta l'approccio di distribuzione tramite una *cartella*, in cui:</span><span class="sxs-lookup"><span data-stu-id="ff765-144">This tutorial adopts the *folder* deployment approach, where:</span></span>

* <span data-ttu-id="ff765-145">L'app viene pubblicata in una cartella.</span><span class="sxs-lookup"><span data-stu-id="ff765-145">The app is published to a folder.</span></span>
* <span data-ttu-id="ff765-146">Il contenuto della cartella viene spostato nella cartella del sito IIS, ovvero il **percorso fisico** del sito in Gestione IIS.</span><span class="sxs-lookup"><span data-stu-id="ff765-146">The folder's contents are moved to the IIS site's folder (the **Physical path** to the site in IIS Manager).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ff765-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ff765-147">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="ff765-148">Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="ff765-148">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span>
1. <span data-ttu-id="ff765-149">Nella finestra di dialogo **Selezionare una destinazione di pubblicazione** selezionare l'opzione di pubblicazione **Cartella**.</span><span class="sxs-lookup"><span data-stu-id="ff765-149">In the **Pick a publish target** dialog, select the **Folder** publish option.</span></span>
1. <span data-ttu-id="ff765-150">Impostare il percorso **Cartella o condivisione file**.</span><span class="sxs-lookup"><span data-stu-id="ff765-150">Set the **Folder or File Share** path.</span></span>
   * <span data-ttu-id="ff765-151">Se è stata creata una cartella per il sito IIS disponibile nel computer di sviluppo come una condivisione di rete, specificare il percorso della condivisione.</span><span class="sxs-lookup"><span data-stu-id="ff765-151">If you created a folder for the IIS site that's available on the development machine as a network share, provide the path to the share.</span></span> <span data-ttu-id="ff765-152">L'utente corrente deve avere l'accesso in scrittura per la pubblicazione nella condivisione.</span><span class="sxs-lookup"><span data-stu-id="ff765-152">The current user must have write access to publish to the share.</span></span>
   * <span data-ttu-id="ff765-153">Se non è possibile eseguire la distribuzione direttamente nella cartella del sito IIS nel server IIS, pubblicare in una cartella su un supporto rimovibile e spostare fisicamente l'app pubblicata nella cartella del sito IIS nel server, ovvero il **percorso fisico** del sito in Gestione IIS.</span><span class="sxs-lookup"><span data-stu-id="ff765-153">If you're unable to deploy directly to the IIS site folder on the IIS server, publish to a folder on removeable media and physically move the published app to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span> <span data-ttu-id="ff765-154">Spostare il contenuto della cartella *bin/Release/{TARGET FRAMEWORK}/publish* nella cartella del sito IIS nel server, ovvero il **percorso fisico** del sito in Gestione IIS.</span><span class="sxs-lookup"><span data-stu-id="ff765-154">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* folder to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ff765-155">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="ff765-155">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="ff765-156">Da una shell dei comandi pubblicare l'app nella configurazione di versione con il comando [dotnet publish](/dotnet/core/tools/dotnet-publish):</span><span class="sxs-lookup"><span data-stu-id="ff765-156">In a command shell, publish the app in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command:</span></span>

   ```dotnetcli
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="ff765-157">Spostare il contenuto della cartella *bin/Release/{TARGET FRAMEWORK}/publish* nella cartella del sito IIS nel server, ovvero il **percorso fisico** del sito in Gestione IIS.</span><span class="sxs-lookup"><span data-stu-id="ff765-157">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* folder to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ff765-158">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="ff765-158">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="ff765-159">Fare clic con il pulsante destro del mouse sul progetto in **Soluzione** e scegliere **Pubblica** > **Pubblica nella cartella**.</span><span class="sxs-lookup"><span data-stu-id="ff765-159">Right-click on the project in **Solution** and select **Publish** > **Publish to Folder**.</span></span>
1. <span data-ttu-id="ff765-160">Impostare il percorso in **Scegliere una cartella**.</span><span class="sxs-lookup"><span data-stu-id="ff765-160">Set the **Choose a folder** path.</span></span>
   * <span data-ttu-id="ff765-161">Se è stata creata una cartella per il sito IIS disponibile nel computer di sviluppo come una condivisione di rete, specificare il percorso della condivisione.</span><span class="sxs-lookup"><span data-stu-id="ff765-161">If you created a folder for the IIS site that's available on the development machine as a network share, provide the path to the share.</span></span> <span data-ttu-id="ff765-162">L'utente corrente deve avere l'accesso in scrittura per la pubblicazione nella condivisione.</span><span class="sxs-lookup"><span data-stu-id="ff765-162">The current user must have write access to publish to the share.</span></span>
   * <span data-ttu-id="ff765-163">Se non è possibile eseguire la distribuzione direttamente nella cartella del sito IIS nel server IIS, pubblicare in una cartella su un supporto rimovibile e spostare fisicamente l'app pubblicata nella cartella del sito IIS nel server, ovvero il **percorso fisico** del sito in Gestione IIS.</span><span class="sxs-lookup"><span data-stu-id="ff765-163">If you're unable to deploy directly to the IIS site folder on the IIS server, publish to a folder on removeable media and physically move the published app to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span> <span data-ttu-id="ff765-164">Spostare il contenuto della cartella *bin/Release/{TARGET FRAMEWORK}/publish* nella cartella del sito IIS nel server, ovvero il **percorso fisico** del sito in Gestione IIS.</span><span class="sxs-lookup"><span data-stu-id="ff765-164">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* folder to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span>

---

## <a name="browse-the-website"></a><span data-ttu-id="ff765-165">Esplorare il sito Web</span><span class="sxs-lookup"><span data-stu-id="ff765-165">Browse the website</span></span>

<span data-ttu-id="ff765-166">L'app è accessibile in un browser dopo la ricezione della prima richiesta.</span><span class="sxs-lookup"><span data-stu-id="ff765-166">The app is accessible in a browser after it receives the first request.</span></span> <span data-ttu-id="ff765-167">Inviare una richiesta all'app nell'associazione dell'endpoint che è stata stabilita in Gestione IIS per il sito.</span><span class="sxs-lookup"><span data-stu-id="ff765-167">Make a request to the app at the endpoint binding that you established in IIS Manager for the site.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ff765-168">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="ff765-168">Next steps</span></span>

<span data-ttu-id="ff765-169">In questa esercitazione si è appreso come:</span><span class="sxs-lookup"><span data-stu-id="ff765-169">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ff765-170">Installare il bundle di hosting di.NET Core in Windows Server.</span><span class="sxs-lookup"><span data-stu-id="ff765-170">Install the .NET Core Hosting Bundle on Windows Server.</span></span>
> * <span data-ttu-id="ff765-171">Creare un sito IIS in Gestione IIS.</span><span class="sxs-lookup"><span data-stu-id="ff765-171">Create an IIS site in IIS Manager.</span></span>
> * <span data-ttu-id="ff765-172">Distribuire un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ff765-172">Deploy an ASP.NET Core app.</span></span>

<span data-ttu-id="ff765-173">Per altre informazioni sull'hosting di app ASP.NET Core in IIS, vedere la panoramica su IIS:</span><span class="sxs-lookup"><span data-stu-id="ff765-173">To learn more about hosting ASP.NET Core apps on IIS, see the IIS Overview article:</span></span>

> [!div class="nextstepaction"]
> <xref:host-and-deploy/iis/index>

## <a name="additional-resources"></a><span data-ttu-id="ff765-174">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="ff765-174">Additional resources</span></span>

### <a name="articles-in-the-aspnet-core-documentation-set"></a><span data-ttu-id="ff765-175">Articoli nel set di documentazione di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ff765-175">Articles in the ASP.NET Core documentation set</span></span>

* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/directory-structure>
* <xref:test/troubleshoot-azure-iis>
* <xref:security/enforcing-ssl>

### <a name="articles-pertaining-to-aspnet-core-app-deployment"></a><span data-ttu-id="ff765-176">Articoli relativi alla distribuzione di app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ff765-176">Articles pertaining to ASP.NET Core app deployment</span></span>

* <xref:tutorials/publish-to-azure-webapp-using-vs>
* <xref:tutorials/publish-to-azure-webapp-using-vscode>
* <xref:host-and-deploy/visual-studio-publish-profiles>
* [<span data-ttu-id="ff765-177">Pubblicare un'app Web in una cartella usando Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="ff765-177">Publish a Web app to a folder using Visual Studio for Mac</span></span>](/visualstudio/mac/publish-folder)

### <a name="articles-on-iis-https-configuration"></a><span data-ttu-id="ff765-178">Articoli sulla configurazione HTTPS di IIS</span><span class="sxs-lookup"><span data-stu-id="ff765-178">Articles on IIS HTTPS configuration</span></span>

* [<span data-ttu-id="ff765-179">Configurazione di SSL in Gestione IIS</span><span class="sxs-lookup"><span data-stu-id="ff765-179">Configuring SSL in IIS Manager</span></span>](/iis/manage/configuring-security/configuring-ssl-in-iis-manager)
* [<span data-ttu-id="ff765-180">Come configurare SSL in IIS</span><span class="sxs-lookup"><span data-stu-id="ff765-180">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)

### <a name="articles-on-iis-and-windows-server"></a><span data-ttu-id="ff765-181">Articoli su IIS e Windows Server</span><span class="sxs-lookup"><span data-stu-id="ff765-181">Articles on IIS and Windows Server</span></span>

* [<span data-ttu-id="ff765-182">Il sito IIS ufficiale di Microsoft</span><span class="sxs-lookup"><span data-stu-id="ff765-182">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="ff765-183">Libreria di contenuti tecnici di Windows Server</span><span class="sxs-lookup"><span data-stu-id="ff765-183">Windows Server technical content library</span></span>](/windows-server/windows-server)
