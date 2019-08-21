---
title: Pubblicare un'app ASP.NET Core in IIS
author: guardrex
description: Informazioni su come ospitare un'app ASP.NET Core in un server IIS.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/08/2019
uid: tutorials/publish-to-iis
ms.openlocfilehash: c31beae16f46153daac188ab1638e5530584ac88
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2019
ms.locfileid: "68927091"
---
# <a name="publish-an-aspnet-core-app-to-iis"></a><span data-ttu-id="81f71-103">Pubblicare un'app ASP.NET Core in IIS</span><span class="sxs-lookup"><span data-stu-id="81f71-103">Publish an ASP.NET Core app to IIS</span></span>

<span data-ttu-id="81f71-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="81f71-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="81f71-105">Questa esercitazione mostra come ospitare un'app ASP.NET Core in un server IIS.</span><span class="sxs-lookup"><span data-stu-id="81f71-105">This tutorial shows how to host an ASP.NET Core app on an IIS server.</span></span>

<span data-ttu-id="81f71-106">Questa esercitazione illustra le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="81f71-106">This tutorial covers the following subjects:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="81f71-107">Installare il bundle di hosting di.NET Core in Windows Server.</span><span class="sxs-lookup"><span data-stu-id="81f71-107">Install the .NET Core Hosting Bundle on Windows Server.</span></span>
> * <span data-ttu-id="81f71-108">Creare un sito IIS in Gestione IIS.</span><span class="sxs-lookup"><span data-stu-id="81f71-108">Create an IIS site in IIS Manager.</span></span>
> * <span data-ttu-id="81f71-109">Distribuire un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="81f71-109">Deploy an ASP.NET Core app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="81f71-110">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="81f71-110">Prerequisites</span></span>

* <span data-ttu-id="81f71-111">[.NET Core SDK](/dotnet/core/sdk) installato nel computer di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="81f71-111">[.NET Core SDK](/dotnet/core/sdk) installed on the development machine.</span></span>
* <span data-ttu-id="81f71-112">Windows Server configurato con il ruolo del server **Server Web (IIS)** .</span><span class="sxs-lookup"><span data-stu-id="81f71-112">Windows Server configured with the **Web Server (IIS)** server role.</span></span> <span data-ttu-id="81f71-113">Se il server non è configurato per ospitare siti Web con IIS, seguire le istruzioni nella sezione *Configurazione di IIS* dell'articolo <xref:host-and-deploy/iis/index#iis-configuration> e quindi tornare a questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="81f71-113">If your server isn't configured to host websites with IIS, follow the guidance in the *IIS configuration* section of the <xref:host-and-deploy/iis/index#iis-configuration> article and then return to this tutorial.</span></span>

> [!WARNING]
> <span data-ttu-id="81f71-114">**La configurazione di IIS e la sicurezza del sito Web coinvolgono concetti non trattati in questa esercitazione.**</span><span class="sxs-lookup"><span data-stu-id="81f71-114">**IIS configuration and website security involve concepts that aren't covered by this tutorial.**</span></span> <span data-ttu-id="81f71-115">Consultare le linee guida per IIS nella [documentazione di Microsoft IIS](https://www.iis.net/) e l'[articolo di ASP.NET Core sull'hosting con IIS](xref:host-and-deploy/iis/index) prima di ospitare app di produzione in IIS.</span><span class="sxs-lookup"><span data-stu-id="81f71-115">Consult the IIS guidance in the [Microsoft IIS documentation](https://www.iis.net/) and the [ASP.NET Core article on hosting with IIS](xref:host-and-deploy/iis/index) before hosting production apps on IIS.</span></span>
>
> <span data-ttu-id="81f71-116">Gli scenari importanti per l'hosting di IIS non trattati in questa esercitazione includono:</span><span class="sxs-lookup"><span data-stu-id="81f71-116">Important scenarios for IIS hosting not covered by this tutorial include:</span></span>
>
> * [<span data-ttu-id="81f71-117">Creazione di un hive del Registro di sistema per la protezione dei dati ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="81f71-117">Creation of a registry hive for ASP.NET Core Data Protection</span></span>](xref:host-and-deploy/iis/index#data-protection)
> * [<span data-ttu-id="81f71-118">Configurazione dell'elenco di controllo di accesso (ACL) del pool di app</span><span class="sxs-lookup"><span data-stu-id="81f71-118">Configuration of the app pool's Access Control List (ACL)</span></span>](xref:host-and-deploy/iis/index#application-pool-identity)
> * <span data-ttu-id="81f71-119">Per concentrarsi sui concetti correlati alla distribuzione di IIS, questa esercitazione illustra come distribuire un'app senza sicurezza HTTPS configurata in IIS.</span><span class="sxs-lookup"><span data-stu-id="81f71-119">To focus on IIS deployment concepts, this tutorial deploys an app without HTTPS security configured in IIS.</span></span> <span data-ttu-id="81f71-120">Per altre informazioni sull'hosting di un'app abilitata per il protocollo HTTPS, vedere gli argomenti relativi alla sicurezza nella sezione [Risorse aggiuntive](#additional-resources) di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="81f71-120">For more information on hosting an app enabled for HTTPS protocol, see the security topics in the [Additional resources](#additional-resources) section of this article.</span></span> <span data-ttu-id="81f71-121">Altre indicazioni per l'hosting di app ASP.NET Core sono disponibili nell'articolo <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="81f71-121">Further guidance for hosting ASP.NET Core apps is provided in the <xref:host-and-deploy/iis/index> article.</span></span>

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="81f71-122">Installare il bundle di hosting .NET Core</span><span class="sxs-lookup"><span data-stu-id="81f71-122">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="81f71-123">Installare il *bundle di hosting .NET Core* nel server IIS.</span><span class="sxs-lookup"><span data-stu-id="81f71-123">Install the *.NET Core Hosting Bundle* on the IIS server.</span></span> <span data-ttu-id="81f71-124">L'aggregazione installa il runtime di .NET Core, la libreria di .NET Core e il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="81f71-124">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="81f71-125">Il modulo consente l'esecuzione delle app ASP.NET Core dietro IIS.</span><span class="sxs-lookup"><span data-stu-id="81f71-125">The module allows ASP.NET Core apps to run behind IIS.</span></span> <span data-ttu-id="81f71-126">Se il sistema non ha una connessione Internet, ottenere e installare [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) prima di installare il bundle di hosting .NET Core.</span><span class="sxs-lookup"><span data-stu-id="81f71-126">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Hosting Bundle.</span></span>

<span data-ttu-id="81f71-127">Scaricare il programma di installazione mediante il collegamento seguente:</span><span class="sxs-lookup"><span data-stu-id="81f71-127">Download the installer using the following link:</span></span>

[<span data-ttu-id="81f71-128">Programma di installazione del bundle di hosting .NET Core corrente (download diretto)</span><span class="sxs-lookup"><span data-stu-id="81f71-128">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

1. <span data-ttu-id="81f71-129">Eseguire il programma di installazione nel server IIS.</span><span class="sxs-lookup"><span data-stu-id="81f71-129">Run the installer on the IIS server.</span></span>

1. <span data-ttu-id="81f71-130">Riavviare il server o eseguire **net stop was /y** seguito da **net start w3svc** in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="81f71-130">Restart the server or execute **net stop was /y** followed by **net start w3svc** in a command shell.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="81f71-131">Creare il sito IIS</span><span class="sxs-lookup"><span data-stu-id="81f71-131">Create the IIS site</span></span>

1. <span data-ttu-id="81f71-132">Nel server IIS creare una cartella per contenere le cartelle e i file pubblicati dell'app.</span><span class="sxs-lookup"><span data-stu-id="81f71-132">On the IIS server, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="81f71-133">In un passaggio successivo, il percorso della cartella viene fornito a IIS come percorso fisico dell'app.</span><span class="sxs-lookup"><span data-stu-id="81f71-133">In a following step, the folder's path is provided to IIS as the physical path to the app.</span></span>

1. <span data-ttu-id="81f71-134">In Gestione IIS aprire il nodo del server nel pannello **Connessioni**.</span><span class="sxs-lookup"><span data-stu-id="81f71-134">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="81f71-135">Fare clic con il pulsante destro del mouse sulla cartella **Siti**.</span><span class="sxs-lookup"><span data-stu-id="81f71-135">Right-click the **Sites** folder.</span></span> <span data-ttu-id="81f71-136">Scegliere **Aggiungi sito Web** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="81f71-136">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="81f71-137">Specificare un **Nome del sito** e impostare il **Percorso fisico** per la cartella di distribuzione dell'app che è stata creata.</span><span class="sxs-lookup"><span data-stu-id="81f71-137">Provide a **Site name** and set the **Physical path** to the app's deployment folder that you created.</span></span> <span data-ttu-id="81f71-138">Specificare la configurazione in **Binding** e creare il sito Web selezionando **OK**.</span><span class="sxs-lookup"><span data-stu-id="81f71-138">Provide the **Binding** configuration and create the website by selecting **OK**.</span></span>

## <a name="create-an-aspnet-core-razor-pages-app"></a><span data-ttu-id="81f71-139">Creare un'app ASP.NET Core Razor Pages</span><span class="sxs-lookup"><span data-stu-id="81f71-139">Create an ASP.NET Core Razor Pages app</span></span>

<span data-ttu-id="81f71-140">Seguire l'esercitazione <xref:getting-started> per creare un'app Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="81f71-140">Follow the <xref:getting-started> tutorial to create a Razor Pages app.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="81f71-141">Pubblicare e distribuire l'app</span><span class="sxs-lookup"><span data-stu-id="81f71-141">Publish and deploy the app</span></span>

<span data-ttu-id="81f71-142">*Pubblicare un'app* significa creare un'app compilata che può essere ospitata da un server.</span><span class="sxs-lookup"><span data-stu-id="81f71-142">*Publish an app* means to produce a compiled app that can be hosted by a server.</span></span> <span data-ttu-id="81f71-143">*Distribuire un'app* significa spostare l'app pubblicata in un sistema di hosting.</span><span class="sxs-lookup"><span data-stu-id="81f71-143">*Deploy an app* means to move the published app to a hosting system.</span></span> <span data-ttu-id="81f71-144">Il passaggio di pubblicazione viene gestito da [.NET Core SDK](/dotnet/core/sdk), mentre la fase di distribuzione può essere gestita tramite vari approcci.</span><span class="sxs-lookup"><span data-stu-id="81f71-144">The publish step is handled by the [.NET Core SDK](/dotnet/core/sdk), while the deployment step can be handled by a variety of approaches.</span></span> <span data-ttu-id="81f71-145">Questa esercitazione adotta l'approccio di distribuzione tramite una *cartella*, in cui:</span><span class="sxs-lookup"><span data-stu-id="81f71-145">This tutorial adopts the *folder* deployment approach, where:</span></span>

* <span data-ttu-id="81f71-146">L'app viene pubblicata in una cartella.</span><span class="sxs-lookup"><span data-stu-id="81f71-146">The app is published to a folder.</span></span>
* <span data-ttu-id="81f71-147">Il contenuto della cartella viene spostato nella cartella del sito IIS, ovvero il **percorso fisico** del sito in Gestione IIS.</span><span class="sxs-lookup"><span data-stu-id="81f71-147">The folder's contents are moved to the IIS site's folder (the **Physical path** to the site in IIS Manager).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="81f71-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="81f71-148">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="81f71-149">Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="81f71-149">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span>
1. <span data-ttu-id="81f71-150">Nella finestra di dialogo **Selezionare una destinazione di pubblicazione** selezionare l'opzione di pubblicazione **Cartella**.</span><span class="sxs-lookup"><span data-stu-id="81f71-150">In the **Pick a publish target** dialog, select the **Folder** publish option.</span></span>
1. <span data-ttu-id="81f71-151">Impostare il percorso **Cartella o condivisione file**.</span><span class="sxs-lookup"><span data-stu-id="81f71-151">Set the **Folder or File Share** path.</span></span>
   * <span data-ttu-id="81f71-152">Se è stata creata una cartella per il sito IIS disponibile nel computer di sviluppo come una condivisione di rete, specificare il percorso della condivisione.</span><span class="sxs-lookup"><span data-stu-id="81f71-152">If you created a folder for the IIS site that's available on the development machine as a network share, provide the path to the share.</span></span> <span data-ttu-id="81f71-153">L'utente corrente deve avere l'accesso in scrittura per la pubblicazione nella condivisione.</span><span class="sxs-lookup"><span data-stu-id="81f71-153">The current user must have write access to publish to the share.</span></span>
   * <span data-ttu-id="81f71-154">Se non è possibile eseguire la distribuzione direttamente nella cartella del sito IIS nel server IIS, pubblicare in una cartella su un supporto rimovibile e spostare fisicamente l'app pubblicata nella cartella del sito IIS nel server, ovvero il **percorso fisico** del sito in Gestione IIS.</span><span class="sxs-lookup"><span data-stu-id="81f71-154">If you're unable to deploy directly to the IIS site folder on the IIS server, publish to a folder on removeable media and physically move the published app to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span> <span data-ttu-id="81f71-155">Spostare il contenuto della cartella *bin/Release/{TARGET FRAMEWORK}/publish* nella cartella del sito IIS nel server, ovvero il **percorso fisico** del sito in Gestione IIS.</span><span class="sxs-lookup"><span data-stu-id="81f71-155">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* folder to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="81f71-156">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="81f71-156">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="81f71-157">Da una shell dei comandi pubblicare l'app nella configurazione di versione con il comando [dotnet publish](/dotnet/core/tools/dotnet-publish):</span><span class="sxs-lookup"><span data-stu-id="81f71-157">In a command shell, publish the app in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="81f71-158">Spostare il contenuto della cartella *bin/Release/{TARGET FRAMEWORK}/publish* nella cartella del sito IIS nel server, ovvero il **percorso fisico** del sito in Gestione IIS.</span><span class="sxs-lookup"><span data-stu-id="81f71-158">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* folder to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="81f71-159">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="81f71-159">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="81f71-160">Fare clic con il pulsante destro del mouse sul progetto in **Soluzione** e scegliere **Pubblica** > **Pubblica nella cartella**.</span><span class="sxs-lookup"><span data-stu-id="81f71-160">Right-click on the project in **Solution** and select **Publish** > **Publish to Folder**.</span></span>
1. <span data-ttu-id="81f71-161">Impostare il percorso in **Scegliere una cartella**.</span><span class="sxs-lookup"><span data-stu-id="81f71-161">Set the **Choose a folder** path.</span></span>
   * <span data-ttu-id="81f71-162">Se è stata creata una cartella per il sito IIS disponibile nel computer di sviluppo come una condivisione di rete, specificare il percorso della condivisione.</span><span class="sxs-lookup"><span data-stu-id="81f71-162">If you created a folder for the IIS site that's available on the development machine as a network share, provide the path to the share.</span></span> <span data-ttu-id="81f71-163">L'utente corrente deve avere l'accesso in scrittura per la pubblicazione nella condivisione.</span><span class="sxs-lookup"><span data-stu-id="81f71-163">The current user must have write access to publish to the share.</span></span>
   * <span data-ttu-id="81f71-164">Se non è possibile eseguire la distribuzione direttamente nella cartella del sito IIS nel server IIS, pubblicare in una cartella su un supporto rimovibile e spostare fisicamente l'app pubblicata nella cartella del sito IIS nel server, ovvero il **percorso fisico** del sito in Gestione IIS.</span><span class="sxs-lookup"><span data-stu-id="81f71-164">If you're unable to deploy directly to the IIS site folder on the IIS server, publish to a folder on removeable media and physically move the published app to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span> <span data-ttu-id="81f71-165">Spostare il contenuto della cartella *bin/Release/{TARGET FRAMEWORK}/publish* nella cartella del sito IIS nel server, ovvero il **percorso fisico** del sito in Gestione IIS.</span><span class="sxs-lookup"><span data-stu-id="81f71-165">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* folder to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span>

---

## <a name="browse-the-website"></a><span data-ttu-id="81f71-166">Esplorare il sito Web</span><span class="sxs-lookup"><span data-stu-id="81f71-166">Browse the website</span></span>

<span data-ttu-id="81f71-167">L'app è accessibile in un browser dopo la ricezione della prima richiesta.</span><span class="sxs-lookup"><span data-stu-id="81f71-167">The app is accessible in a browser after it receives the first request.</span></span> <span data-ttu-id="81f71-168">Inviare una richiesta all'app nell'associazione dell'endpoint che è stata stabilita in Gestione IIS per il sito.</span><span class="sxs-lookup"><span data-stu-id="81f71-168">Make a request to the app at the endpoint binding that you established in IIS Manager for the site.</span></span>

## <a name="next-steps"></a><span data-ttu-id="81f71-169">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="81f71-169">Next steps</span></span>

<span data-ttu-id="81f71-170">In questa esercitazione si è appreso come:</span><span class="sxs-lookup"><span data-stu-id="81f71-170">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="81f71-171">Installare il bundle di hosting di.NET Core in Windows Server.</span><span class="sxs-lookup"><span data-stu-id="81f71-171">Install the .NET Core Hosting Bundle on Windows Server.</span></span>
> * <span data-ttu-id="81f71-172">Creare un sito IIS in Gestione IIS.</span><span class="sxs-lookup"><span data-stu-id="81f71-172">Create an IIS site in IIS Manager.</span></span>
> * <span data-ttu-id="81f71-173">Distribuire un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="81f71-173">Deploy an ASP.NET Core app.</span></span>

<span data-ttu-id="81f71-174">Per altre informazioni sull'hosting di app ASP.NET Core in IIS, vedere la panoramica su IIS:</span><span class="sxs-lookup"><span data-stu-id="81f71-174">To learn more about hosting ASP.NET Core apps on IIS, see the IIS Overview article:</span></span>

> [!div class="nextstepaction"]
> <xref:host-and-deploy/iis/index>

## <a name="additional-resources"></a><span data-ttu-id="81f71-175">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="81f71-175">Additional resources</span></span>

### <a name="articles-in-the-aspnet-core-documentation-set"></a><span data-ttu-id="81f71-176">Articoli nel set di documentazione di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="81f71-176">Articles in the ASP.NET Core documentation set</span></span>

* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/directory-structure>
* <xref:test/troubleshoot-azure-iis>
* <xref:security/enforcing-ssl>

### <a name="articles-pertaining-to-aspnet-core-app-deployment"></a><span data-ttu-id="81f71-177">Articoli relativi alla distribuzione di app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="81f71-177">Articles pertaining to ASP.NET Core app deployment</span></span>

* <xref:tutorials/publish-to-azure-webapp-using-vs>
* <xref:tutorials/publish-to-azure-webapp-using-vscode>
* <xref:host-and-deploy/visual-studio-publish-profiles>
* [<span data-ttu-id="81f71-178">Pubblicare un'app Web in una cartella usando Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="81f71-178">Publish a Web app to a folder using Visual Studio for Mac</span></span>](/visualstudio/mac/publish-folder)

### <a name="articles-on-iis-https-configuration"></a><span data-ttu-id="81f71-179">Articoli sulla configurazione HTTPS di IIS</span><span class="sxs-lookup"><span data-stu-id="81f71-179">Articles on IIS HTTPS configuration</span></span>

* [<span data-ttu-id="81f71-180">Configurazione di SSL in Gestione IIS</span><span class="sxs-lookup"><span data-stu-id="81f71-180">Configuring SSL in IIS Manager</span></span>](/iis/manage/configuring-security/configuring-ssl-in-iis-manager)
* [<span data-ttu-id="81f71-181">Come configurare SSL in IIS</span><span class="sxs-lookup"><span data-stu-id="81f71-181">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)

### <a name="articles-on-iis-and-windows-server"></a><span data-ttu-id="81f71-182">Articoli su IIS e Windows Server</span><span class="sxs-lookup"><span data-stu-id="81f71-182">Articles on IIS and Windows Server</span></span>

* [<span data-ttu-id="81f71-183">Il sito IIS ufficiale di Microsoft</span><span class="sxs-lookup"><span data-stu-id="81f71-183">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="81f71-184">Libreria di contenuti tecnici di Windows Server</span><span class="sxs-lookup"><span data-stu-id="81f71-184">Windows Server technical content library</span></span>](/windows-server/windows-server)
