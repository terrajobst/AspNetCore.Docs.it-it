---
title: Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core
author: guardrex
description: Informazioni sul supporto del debug di app ASP.NET Core durante l'esecuzione con IIS in Windows Server.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 6f555858239b4432d252f8b3ac7add5c3e8bfe62
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59425101"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="c5ca2-103">Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c5ca2-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="c5ca2-104">Di [Sourabh Shirhatti](https://twitter.com/sshirhatti) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c5ca2-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c5ca2-105">Questo articolo descrive il supporto del debug di app ASP.NET Core in [Visual Studio](https://www.visualstudio.com/vs/) durante l'esecuzione con IIS in Windows Server.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running with IIS on Windows Server.</span></span> <span data-ttu-id="c5ca2-106">Questo argomento illustra nel dettaglio l'abilitazione di questo scenario e la configurazione di un progetto.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-106">This topic walks through enabling this scenario and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c5ca2-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="c5ca2-107">Prerequisites</span></span>

* [<span data-ttu-id="c5ca2-108">Visual Studio per Windows</span><span class="sxs-lookup"><span data-stu-id="c5ca2-108">Visual Studio for Windows</span></span>](https://visualstudio.microsoft.com/downloads/)
* <span data-ttu-id="c5ca2-109">**ASP.NET e carico di lavoro di sviluppo Web**</span><span class="sxs-lookup"><span data-stu-id="c5ca2-109">**ASP.NET and web development** workload</span></span>
* <span data-ttu-id="c5ca2-110">Carico di lavoro di **sviluppo multipiattaforma .NET Core**</span><span class="sxs-lookup"><span data-stu-id="c5ca2-110">**.NET Core cross-platform development** workload</span></span>
* <span data-ttu-id="c5ca2-111">Certificato di sicurezza X.509 (per il supporto di HTTPS)</span><span class="sxs-lookup"><span data-stu-id="c5ca2-111">X.509 security certificate (for HTTPS support)</span></span>

## <a name="enable-iis"></a><span data-ttu-id="c5ca2-112">Abilitare IIS</span><span class="sxs-lookup"><span data-stu-id="c5ca2-112">Enable IIS</span></span>

1. <span data-ttu-id="c5ca2-113">In Windows passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo).</span><span class="sxs-lookup"><span data-stu-id="c5ca2-113">In Windows, navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="c5ca2-114">Selezionare la casella di controllo **Internet Information Services**.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-114">Select the **Internet Information Services** check box.</span></span> <span data-ttu-id="c5ca2-115">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-115">Select **OK**.</span></span>

<span data-ttu-id="c5ca2-116">L'installazione di IIS potrebbe richiedere un riavvio del sistema.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-116">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="c5ca2-117">Configura IIS</span><span class="sxs-lookup"><span data-stu-id="c5ca2-117">Configure IIS</span></span>

<span data-ttu-id="c5ca2-118">IIS deve disporre di un sito Web configurato con gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c5ca2-118">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="c5ca2-119">**Nome host** &ndash; In genere viene usato il valore **Sito Web predefinito** come **Nome host** di `localhost`.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-119">**Host name** &ndash; Typically, the **Default Web Site** is used with a **Host name** of `localhost`.</span></span> <span data-ttu-id="c5ca2-120">Tuttavia, è appropriato qualsiasi sito Web IIS valido con un nome host univoco.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-120">However, any valid IIS website with a unique host name works.</span></span>
* <span data-ttu-id="c5ca2-121">**Binding del sito**</span><span class="sxs-lookup"><span data-stu-id="c5ca2-121">**Site Binding**</span></span>
  * <span data-ttu-id="c5ca2-122">Per le app che richiedono HTTPS, creare un binding alla porta 443 con un certificato.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-122">For apps that require HTTPS, create a binding to port 443 with a certificate.</span></span> <span data-ttu-id="c5ca2-123">Viene in genere usato il **certificato di sviluppo di IIS Express**, ma può essere usato qualsiasi certificato valido.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-123">Typically, the **IIS Express Development Certificate** is used, but any valid certificate works.</span></span>
  * <span data-ttu-id="c5ca2-124">Per le app che usano HTTP, verificare l'esistenza di un binding alla porta 80 o creare un binding alla porta 80 per un nuovo sito.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-124">For apps that use HTTP, confirm the existence of a binding to post 80 or create a binding to port 80 for a new site.</span></span>
  * <span data-ttu-id="c5ca2-125">Usare un singolo binding per HTTP o HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-125">Use a single binding for either HTTP or HTTPS.</span></span> <span data-ttu-id="c5ca2-126">**Non è supportato il binding a porte HTTP e HTTPS in contemporanea.**</span><span class="sxs-lookup"><span data-stu-id="c5ca2-126">**Binding to both HTTP and HTTPS ports simultaneously isn't supported.**</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="c5ca2-127">Abilitare il supporto di IIS in fase di sviluppo in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c5ca2-127">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="c5ca2-128">Avviare il programma di installazione di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-128">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="c5ca2-129">Selezionare **Modifica** per l'installazione di Visual Studio che si intende usare per il supporto di IIS in fase di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-129">Select **Modify** for the Visual Studio installation that you plan to use for IIS development-time support.</span></span>
1. <span data-ttu-id="c5ca2-130">Per il carico di lavoro **Sviluppo ASP.NET e Web**, individuare e installare il componente **Supporto IIS in fase di sviluppo**.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-130">For the **ASP.NET and web development** workload, locate and install the **Development time IIS support** component.</span></span>

   <span data-ttu-id="c5ca2-131">Il componente è elencato nella sezione **Facoltativo** in **Supporto IIS in fase di sviluppo** nel pannello **Dettagli di installazione** a destra dei carichi di lavoro.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-131">The component is listed in the **Optional** section under **Development time IIS support** in the **Installation details** panel to the right of the workloads.</span></span> <span data-ttu-id="c5ca2-132">Il componente installa il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module), un modulo IIS nativo necessario per l'esecuzione delle applicazioni ASP.NET Core con IIS.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-132">The component installs the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps with IIS.</span></span>

## <a name="configure-the-project"></a><span data-ttu-id="c5ca2-133">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="c5ca2-133">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="c5ca2-134">Reindirizzamento HTTPS</span><span class="sxs-lookup"><span data-stu-id="c5ca2-134">HTTPS redirection</span></span>

<span data-ttu-id="c5ca2-135">Per un nuovo progetto che richiede HTTPS, selezionare la casella di controllo **Configura per HTTPS** nella finestra **Crea una nuova applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-135">For a new project that requires HTTPS, select the check box to **Configure for HTTPS** in the **Create a new ASP.NET Core Web Application** window.</span></span> <span data-ttu-id="c5ca2-136">Selezionando questa casella di controllo viene aggiunto il [middleware di reindirizzamento HTTPS e HSTS](xref:security/enforcing-ssl) all'app quando viene creata.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-136">Selecting the check box adds [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) to the app when it's created.</span></span>

<span data-ttu-id="c5ca2-137">Per un progetto esistente che richiede HTTPS, usare il middleware di reindirizzamento HTTPS e HSTS in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-137">For an existing project that requires HTTPS, use HTTPS Redirection and HSTS Middleware in `Startup.Configure`.</span></span> <span data-ttu-id="c5ca2-138">Per ulteriori informazioni, vedere <xref:security/enforcing-ssl>.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-138">For more information, see <xref:security/enforcing-ssl>.</span></span>

<span data-ttu-id="c5ca2-139">Per un progetto che usa HTTP, il [middleware di reindirizzamento HTTPS e HSTS](xref:security/enforcing-ssl) non viene aggiunto all'app.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-139">For a project that uses HTTP, [HTTPS Redirection and HSTS Middleware](xref:security/enforcing-ssl) aren't added to the app.</span></span> <span data-ttu-id="c5ca2-140">Non è richiesta alcuna configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-140">No app configuration is required.</span></span>

### <a name="iis-launch-profile"></a><span data-ttu-id="c5ca2-141">Profilo di avvio IIS</span><span class="sxs-lookup"><span data-stu-id="c5ca2-141">IIS launch profile</span></span>

<span data-ttu-id="c5ca2-142">Creare un nuovo profilo di avvio per aggiungere il supporto IIS in fase di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="c5ca2-142">Create a new launch profile to add development-time IIS support:</span></span>

::: moniker range=">= aspnetcore-3.0"

1. <span data-ttu-id="c5ca2-143">Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni**.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-143">Right-click the project in **Solution Explorer**.</span></span> <span data-ttu-id="c5ca2-144">Selezionare **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-144">Select **Properties**.</span></span> <span data-ttu-id="c5ca2-145">Aprire la scheda **Debug**.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-145">Open the **Debug** tab.</span></span>
1. <span data-ttu-id="c5ca2-146">Per **Profilo** selezionare il pulsante **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-146">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="c5ca2-147">Denominare il profilo "IIS" nella finestra popup.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-147">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="c5ca2-148">Selezionare **OK** per creare il profilo.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-148">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="c5ca2-149">Per l'impostazione **Avvio** selezionare **IIS** dall'elenco.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-149">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="c5ca2-150">Selezionare la casella di controllo **Avvia browser** e specificare l'URL dell'endpoint.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-150">Select the check box for **Launch browser** and provide the endpoint URL.</span></span>

   <span data-ttu-id="c5ca2-151">Quando l'app richiede HTTPS, usare un endpoint HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="c5ca2-151">When the app requires HTTPS, use an HTTPS endpoint (`https://`).</span></span> <span data-ttu-id="c5ca2-152">Per il protocollo HTTP, usare un endpoint HTTP (`http://`).</span><span class="sxs-lookup"><span data-stu-id="c5ca2-152">For HTTP, use an HTTP (`http://`) endpoint.</span></span>

   <span data-ttu-id="c5ca2-153">Specificare lo stesso nome host e la stessa porta usati nella [configurazione di IIS specificata in precedenza](#configure-iis), in genere `localhost`.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-153">Provide the same host name and port as the [IIS configuration specified earlier uses](#configure-iis), typically `localhost`.</span></span>

   <span data-ttu-id="c5ca2-154">Specificare il nome dell'app alla fine dell'URL.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-154">Provide the name of the app at the end of the URL.</span></span>

   <span data-ttu-id="c5ca2-155">Ad esempio, `https://localhost/WebApplication1` (HTTPS) o `http://localhost/WebApplication1` (HTTP) sono URL di endpoint validi.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-155">For example, `https://localhost/WebApplication1` (HTTPS) or `http://localhost/WebApplication1` (HTTP) are valid endpoint URLs.</span></span>
1. <span data-ttu-id="c5ca2-156">Nella sezione **Variabili di ambiente** selezionare il pulsante **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-156">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="c5ca2-157">Specificare una variabile di ambiente con **Nome** `ASPNETCORE_ENVIRONMENT` e **Valore** `Development`.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-157">Provide an environment variable with a **Name** of `ASPNETCORE_ENVIRONMENT` and a **Value** of `Development`.</span></span>
1. <span data-ttu-id="c5ca2-158">Nell'area **Impostazioni server Web** impostare **URL app** sullo stesso valore usato per l'URL dell'endpoint **Avvia browser**.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-158">In the **Web Server Settings** area, set the **App URL** to the same value used for the **Launch browser** endpoint URL.</span></span>
1. <span data-ttu-id="c5ca2-159">Per l'impostazione **Modello di hosting** in Visual Studio 2019 o versione successiva, selezionare **Predefinito** per usare il modello di hosting usato dal progetto.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-159">For the **Hosting Model** setting in Visual Studio 2019 or later, select **Default** to use the hosting model used by the project.</span></span> <span data-ttu-id="c5ca2-160">Se il progetto imposta la proprietà `<AspNetCoreHostingModel>` nel file di progetto, viene usato il valore della proprietà (`InProcess` o `OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="c5ca2-160">If the project sets the `<AspNetCoreHostingModel>` property in its project file, the value of the property (`InProcess` or `OutOfProcess`) is used.</span></span> <span data-ttu-id="c5ca2-161">Se la proprietà non è presente, viene usato il modello di hosting predefinito dell'app, ovvero in-process.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-161">If the property isn't present, the default hosting model of the app is used, which is in-process.</span></span> <span data-ttu-id="c5ca2-162">Se l'app richiede l'impostazione esplicita di un modello di hosting diverso dal modello di hosting normale dell'app, impostare **Modello di hosting** su `In Process` o `Out Of Process` in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-162">If the app requires an explicit hosting model setting different from the app's normal hosting model, set the **Hosting Model** to either `In Process` or `Out Of Process` as needed.</span></span>
1. <span data-ttu-id="c5ca2-163">Salvare il profilo.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-163">Save the profile.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

1. <span data-ttu-id="c5ca2-164">Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni**.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-164">Right-click the project in **Solution Explorer**.</span></span> <span data-ttu-id="c5ca2-165">Selezionare **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-165">Select **Properties**.</span></span> <span data-ttu-id="c5ca2-166">Aprire la scheda **Debug**.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-166">Open the **Debug** tab.</span></span>
1. <span data-ttu-id="c5ca2-167">Per **Profilo** selezionare il pulsante **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-167">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="c5ca2-168">Denominare il profilo "IIS" nella finestra popup.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-168">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="c5ca2-169">Selezionare **OK** per creare il profilo.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-169">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="c5ca2-170">Per l'impostazione **Avvio** selezionare **IIS** dall'elenco.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-170">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="c5ca2-171">Selezionare la casella di controllo **Avvia browser** e specificare l'URL dell'endpoint.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-171">Select the check box for **Launch browser** and provide the endpoint URL.</span></span>

   <span data-ttu-id="c5ca2-172">Quando l'app richiede HTTPS, usare un endpoint HTTPS (`https://`).</span><span class="sxs-lookup"><span data-stu-id="c5ca2-172">When the app requires HTTPS, use an HTTPS endpoint (`https://`).</span></span> <span data-ttu-id="c5ca2-173">Per il protocollo HTTP, usare un endpoint HTTP (`http://`).</span><span class="sxs-lookup"><span data-stu-id="c5ca2-173">For HTTP, use an HTTP (`http://`) endpoint.</span></span>

   <span data-ttu-id="c5ca2-174">Specificare lo stesso nome host e la stessa porta usati nella [configurazione di IIS specificata in precedenza](#configure-iis), in genere `localhost`.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-174">Provide the same host name and port as the [IIS configuration specified earlier uses](#configure-iis), typically `localhost`.</span></span>

   <span data-ttu-id="c5ca2-175">Specificare il nome dell'app alla fine dell'URL.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-175">Provide the name of the app at the end of the URL.</span></span>

   <span data-ttu-id="c5ca2-176">Ad esempio, `https://localhost/WebApplication1` (HTTPS) o `http://localhost/WebApplication1` (HTTP) sono URL di endpoint validi.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-176">For example, `https://localhost/WebApplication1` (HTTPS) or `http://localhost/WebApplication1` (HTTP) are valid endpoint URLs.</span></span>
1. <span data-ttu-id="c5ca2-177">Nella sezione **Variabili di ambiente** selezionare il pulsante **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-177">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="c5ca2-178">Specificare una variabile di ambiente con **Nome** `ASPNETCORE_ENVIRONMENT` e **Valore** `Development`.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-178">Provide an environment variable with a **Name** of `ASPNETCORE_ENVIRONMENT` and a **Value** of `Development`.</span></span>
1. <span data-ttu-id="c5ca2-179">Nell'area **Impostazioni server Web** impostare **URL app** sullo stesso valore usato per l'URL dell'endpoint **Avvia browser**.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-179">In the **Web Server Settings** area, set the **App URL** to the same value used for the **Launch browser** endpoint URL.</span></span>
1. <span data-ttu-id="c5ca2-180">Per l'impostazione **Modello di hosting** in Visual Studio 2019 o versione successiva, selezionare **Predefinito** per usare il modello di hosting usato dal progetto.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-180">For the **Hosting Model** setting in Visual Studio 2019 or later, select **Default** to use the hosting model used by the project.</span></span> <span data-ttu-id="c5ca2-181">Se il progetto imposta la proprietà `<AspNetCoreHostingModel>` nel file di progetto, viene usato il valore della proprietà (`InProcess` o `OutOfProcess`).</span><span class="sxs-lookup"><span data-stu-id="c5ca2-181">If the project sets the `<AspNetCoreHostingModel>` property in its project file, the value of the property (`InProcess` or `OutOfProcess`) is used.</span></span> <span data-ttu-id="c5ca2-182">Se la proprietà non è presente, viene usato il modello di hosting predefinito dell'app, ovvero out-of-process.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-182">If the property isn't present, the default hosting model of the app is used, which is out-of-process.</span></span> <span data-ttu-id="c5ca2-183">Se l'app richiede l'impostazione esplicita di un modello di hosting diverso dal modello di hosting normale dell'app, impostare **Modello di hosting** su `In Process` o `Out Of Process` in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-183">If the app requires an explicit hosting model setting different from the app's normal hosting model, set the **Hosting Model** to either `In Process` or `Out Of Process` as needed.</span></span>
1. <span data-ttu-id="c5ca2-184">Salvare il profilo.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-184">Save the profile.</span></span>

::: moniker-end

<span data-ttu-id="c5ca2-185">Se non si usa Visual Studio, aggiungere manualmente un profilo di avvio al file [launchSettings.json](http://json.schemastore.org/launchsettings) nella cartella *Proprietà*.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-185">When not using Visual Studio, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the *Properties* folder.</span></span> <span data-ttu-id="c5ca2-186">L'esempio seguente configura il profilo per usare il protocollo HTTPS:</span><span class="sxs-lookup"><span data-stu-id="c5ca2-186">The following example configures the profile to use the HTTPS protocol:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iis": {
      "applicationUrl": "https://localhost/WebApplication1",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS": {
      "commandName": "IIS",
      "launchBrowser": true,
      "launchUrl": "https://localhost/WebApplication1",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

<span data-ttu-id="c5ca2-187">Verificare che gli endpoint `applicationUrl` e `launchUrl` corrispondano e usare lo stesso protocollo della configurazione del binding IIS, ovvero HTTP o HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-187">Confirm that the `applicationUrl` and `launchUrl` endpoints match and use the same protocol as the IIS binding configuration, either HTTP or HTTPS.</span></span>

## <a name="run-the-project"></a><span data-ttu-id="c5ca2-188">Eseguire il progetto</span><span class="sxs-lookup"><span data-stu-id="c5ca2-188">Run the project</span></span>

<span data-ttu-id="c5ca2-189">Eseguire Visual Studio come amministratore:</span><span class="sxs-lookup"><span data-stu-id="c5ca2-189">Run Visual Studio as an administrator:</span></span>

* <span data-ttu-id="c5ca2-190">Verificare che l'elenco di riepilogo a discesa della configurazione della build sia impostato su **Debug**.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-190">Confirm that the build configuration drop-down list is set to **Debug**.</span></span>
* <span data-ttu-id="c5ca2-191">Impostare il pulsante Esegui sul profilo **IIS** e selezionare il pulsante per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-191">Set the Run button to the **IIS** profile and select the button to start the app.</span></span>

<span data-ttu-id="c5ca2-192">Visual Studio potrebbe richiedere un riavvio, se non si è in modalità amministratore.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-192">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="c5ca2-193">In tal caso riavviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-193">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="c5ca2-194">Se viene usato un certificato di sviluppo non attendibile, il browser potrebbe richiedere di creare un'eccezione per il certificato non attendibile.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-194">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="c5ca2-195">Il debug della configurazione della build Versione con [Just My Code](/visualstudio/debugger/just-my-code) e le ottimizzazioni del compilatore genera un'esperienza con funzionalità ridotta.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-195">Debugging a Release build configuration with [Just My Code](/visualstudio/debugger/just-my-code) and compiler optimizations results in a degraded experience.</span></span> <span data-ttu-id="c5ca2-196">Ad esempio, i punti di interruzione non vengono raggiunti.</span><span class="sxs-lookup"><span data-stu-id="c5ca2-196">For example, break points aren't hit.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c5ca2-197">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c5ca2-197">Additional resources</span></span>

* <span data-ttu-id="c5ca2-198">[Getting Started with the IIS Manager in IIS](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8) (Introduzione a Gestione IIS in IIS)</span><span class="sxs-lookup"><span data-stu-id="c5ca2-198">[Getting Started with the IIS Manager in IIS](/iis/get-started/getting-started-with-iis/getting-started-with-the-iis-manager-in-iis-7-and-iis-8)</span></span>
* [<span data-ttu-id="c5ca2-199">Host ASP.NET Core in Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="c5ca2-199">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="c5ca2-200">Introduzione al modulo di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c5ca2-200">Introduction to ASP.NET Core Module</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="c5ca2-201">Guida di riferimento per la configurazione del modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c5ca2-201">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="c5ca2-202">Imporre HTTPS</span><span class="sxs-lookup"><span data-stu-id="c5ca2-202">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
