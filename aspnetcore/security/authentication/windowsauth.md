---
title: Configurare l'autenticazione di Windows in ASP.NET Core
author: scottaddie
description: Informazioni su come configurare l'autenticazione di Windows in ASP.NET Core, usando HTTP. sys, IIS e IIS Express.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 02/25/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 15fc41efba77f88fc8129f875b85836ac1b5f886
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833696"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="a6eba-103">Configurare l'autenticazione di Windows in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a6eba-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="a6eba-104">Dal [Scott Addie](https://twitter.com/Scott_Addie) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a6eba-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a6eba-105">[L'autenticazione di Windows](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) può essere configurato per le app ASP.NET Core ospitate con [IIS](xref:host-and-deploy/iis/index) oppure [HTTP. sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="a6eba-105">[Windows Authentication](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="a6eba-106">L'autenticazione di Windows si basa sul sistema operativo per autenticare gli utenti delle App ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a6eba-106">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="a6eba-107">È possibile usare l'autenticazione di Windows quando il server in esecuzione in una rete aziendale usando le identità di dominio Active Directory o account di Windows per identificare gli utenti.</span><span class="sxs-lookup"><span data-stu-id="a6eba-107">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="a6eba-108">L'autenticazione di Windows è più adatta agli ambienti intranet in cui gli utenti, le app client e server web appartengono allo stesso dominio di Windows.</span><span class="sxs-lookup"><span data-stu-id="a6eba-108">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="a6eba-109">Abilitare l'autenticazione di Windows in un'app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a6eba-109">Enable Windows Authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="a6eba-110">Il **applicazione Web** modello disponibile tramite la CLI di .NET Core o Visual Studio possono essere configurato per supportare l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="a6eba-110">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a6eba-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6eba-111">Visual Studio</span></span>](#tab/visual-studio)

### <a name="use-the-windows-authentication-app-template-for-a-new-project"></a><span data-ttu-id="a6eba-112">Usare il modello di app di autenticazione di Windows per un nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="a6eba-112">Use the Windows Authentication app template for a new project</span></span>

<span data-ttu-id="a6eba-113">In Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="a6eba-113">In Visual Studio:</span></span>

1. <span data-ttu-id="a6eba-114">Creare una nuova **applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a6eba-114">Create a new **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="a6eba-115">Selezionare **applicazione Web** dall'elenco dei modelli.</span><span class="sxs-lookup"><span data-stu-id="a6eba-115">Select **Web Application** from the list of templates.</span></span>
1. <span data-ttu-id="a6eba-116">Selezionare il **Modifica autenticazione** e selezionare **l'autenticazione di Windows**.</span><span class="sxs-lookup"><span data-stu-id="a6eba-116">Select the **Change Authentication** button and select **Windows Authentication**.</span></span>

<span data-ttu-id="a6eba-117">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="a6eba-117">Run the app.</span></span> <span data-ttu-id="a6eba-118">Il nome utente viene visualizzato nell'interfaccia utente dell'app sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="a6eba-118">The username appears in the rendered app's user interface.</span></span>

### <a name="manual-configuration-for-an-existing-project"></a><span data-ttu-id="a6eba-119">Configurazione manuale per un progetto esistente</span><span class="sxs-lookup"><span data-stu-id="a6eba-119">Manual configuration for an existing project</span></span>

<span data-ttu-id="a6eba-120">Le proprietà del progetto consentono di abilitare l'autenticazione di Windows e disabilitare l'autenticazione anonima:</span><span class="sxs-lookup"><span data-stu-id="a6eba-120">The project's properties allow you to enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="a6eba-121">Fare clic sul progetto in Visual Studio **Esplora soluzioni** e selezionare **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="a6eba-121">Right-click the project in Visual Studio's **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="a6eba-122">Selezionare la scheda **Debug**.</span><span class="sxs-lookup"><span data-stu-id="a6eba-122">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="a6eba-123">Deselezionare la casella di controllo **abilitare l'autenticazione anonima**.</span><span class="sxs-lookup"><span data-stu-id="a6eba-123">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="a6eba-124">Selezionare la casella di controllo **abilitare l'autenticazione di Windows**.</span><span class="sxs-lookup"><span data-stu-id="a6eba-124">Select the check box for **Enable Windows Authentication**.</span></span>

<span data-ttu-id="a6eba-125">In alternativa, è possibile configurare le proprietà nel `iisSettings` nodo il *launchsettings. JSON* file:</span><span class="sxs-lookup"><span data-stu-id="a6eba-125">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a6eba-126">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="a6eba-126">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a6eba-127">Usare la **Windows autenticazione** modello di app.</span><span class="sxs-lookup"><span data-stu-id="a6eba-127">Use the **Windows Authentication** app template.</span></span>

<span data-ttu-id="a6eba-128">Eseguire la [dotnet nuove](/dotnet/core/tools/dotnet-new) con il `webapp` argomento (App Web di ASP.NET Core) e `--auth Windows` passare:</span><span class="sxs-lookup"><span data-stu-id="a6eba-128">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

---

<span data-ttu-id="a6eba-129">Quando si modifica un progetto esistente, verificare che il file di progetto include un riferimento al pacchetto per il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) **oppure** il [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="a6eba-129">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="a6eba-130">Abilitare l'autenticazione di Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="a6eba-130">Enable Windows Authentication with IIS</span></span>

<span data-ttu-id="a6eba-131">IIS Usa il [modulo di ASP.NET Core](xref:host-and-deploy/aspnet-core-module) per ospitare App ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a6eba-131">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="a6eba-132">L'autenticazione di Windows è configurato per IIS tramite il *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="a6eba-132">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="a6eba-133">Le sezioni seguenti mostrano come:</span><span class="sxs-lookup"><span data-stu-id="a6eba-133">The following sections show how to:</span></span>

* <span data-ttu-id="a6eba-134">Specificare una variabile locale *Web. config* file che attiva l'autenticazione di Windows nel server quando l'app viene distribuita.</span><span class="sxs-lookup"><span data-stu-id="a6eba-134">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="a6eba-135">Usare Gestione IIS per configurare il *Web. config* file di un'app ASP.NET Core che è già stata distribuita nel server.</span><span class="sxs-lookup"><span data-stu-id="a6eba-135">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

### <a name="iis-configuration"></a><span data-ttu-id="a6eba-136">Configurazione di IIS</span><span class="sxs-lookup"><span data-stu-id="a6eba-136">IIS configuration</span></span>

<span data-ttu-id="a6eba-137">Se non già stato fatto, abilitare IIS per ospitare App ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a6eba-137">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="a6eba-138">Per altre informazioni, vedere <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="a6eba-138">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="a6eba-139">Abilitare il servizio ruolo IIS per l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="a6eba-139">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="a6eba-140">Per altre informazioni, vedere [abilitare l'autenticazione di Windows nei servizi di ruolo IIS (vedere il passaggio 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="a6eba-140">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="a6eba-141">Per impostazione predefinita, il Middleware di integrazione IIS è configurato per autenticare automaticamente le richieste.</span><span class="sxs-lookup"><span data-stu-id="a6eba-141">IIS Integration Middleware is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="a6eba-142">Per altre informazioni, vedere [Host ASP.NET Core in Windows con IIS: Le opzioni di IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="a6eba-142">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="a6eba-143">Per impostazione predefinita, il modulo ASP.NET Core è configurato per inoltrare il token di autenticazione di Windows per l'app.</span><span class="sxs-lookup"><span data-stu-id="a6eba-143">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="a6eba-144">Per altre informazioni, vedere [riferimento configurazione modulo ASP.NET Core: Attributi dell'elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="a6eba-144">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="a6eba-145">Creare un nuovo sito IIS</span><span class="sxs-lookup"><span data-stu-id="a6eba-145">Create a new IIS site</span></span>

<span data-ttu-id="a6eba-146">Specificare un nome e una cartella e consentirgli di creare un nuovo pool di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="a6eba-146">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="enable-windows-authentication-for-the-app-in-iis"></a><span data-ttu-id="a6eba-147">Abilitare l'autenticazione di Windows per le app in IIS.</span><span class="sxs-lookup"><span data-stu-id="a6eba-147">Enable Windows Authentication for the app in IIS</span></span>

<span data-ttu-id="a6eba-148">Uso **entrambi** degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="a6eba-148">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="a6eba-149">[Configurazione lato sviluppo prima di pubblicare l'app](#development-side-configuration-with-a-local-webconfig-file) (*consigliato*)</span><span class="sxs-lookup"><span data-stu-id="a6eba-149">[Development-side configuration before publishing the app](#development-side-configuration-with-a-local-webconfig-file) (*Recommended*)</span></span>
* [<span data-ttu-id="a6eba-150">Configurazione lato server dopo aver pubblicato l'app</span><span class="sxs-lookup"><span data-stu-id="a6eba-150">Server-side configuration after publishing the app</span></span>](#server-side-configuration-with-the-iis-manager)

#### <a name="development-side-configuration-with-a-local-webconfig-file"></a><span data-ttu-id="a6eba-151">Configurazione di sviluppo-side con un file Web. config locale</span><span class="sxs-lookup"><span data-stu-id="a6eba-151">Development-side configuration with a local web.config file</span></span>

<span data-ttu-id="a6eba-152">Eseguire la procedura seguente **prima** si [pubblicare e distribuire il progetto](#publish-and-deploy-your-project-to-the-iis-site-folder).</span><span class="sxs-lookup"><span data-stu-id="a6eba-152">Perform the following steps **before** you [publish and deploy your project](#publish-and-deploy-your-project-to-the-iis-site-folder).</span></span>

<span data-ttu-id="a6eba-153">Aggiungere il codice seguente *Web. config* file alla radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="a6eba-153">Add the following *web.config* file to the project root:</span></span>

[!code-xml[](windowsauth/sample_snapshot/web_2.config)]

<span data-ttu-id="a6eba-154">Quando il progetto viene pubblicato dal SDK (senza il `<IsTransformWebConfigDisabled>` impostata su `true` nel file di progetto), pubblicato *Web. config* file include il `<location><system.webServer><security><authentication>` sezione.</span><span class="sxs-lookup"><span data-stu-id="a6eba-154">When the project is published by the SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="a6eba-155">Per altre informazioni sul `<IsTransformWebConfigDisabled>` proprietà, vedere <xref:host-and-deploy/iis/index#webconfig-file>.</span><span class="sxs-lookup"><span data-stu-id="a6eba-155">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

#### <a name="server-side-configuration-with-the-iis-manager"></a><span data-ttu-id="a6eba-156">Configurazione lato server con Gestione IIS</span><span class="sxs-lookup"><span data-stu-id="a6eba-156">Server-side configuration with the IIS Manager</span></span>

<span data-ttu-id="a6eba-157">Eseguire la procedura seguente **dopo aver** si [pubblicare e distribuire il progetto](#publish-and-deploy-your-project-to-the-iis-site-folder).</span><span class="sxs-lookup"><span data-stu-id="a6eba-157">Perform the following steps **after** you [publish and deploy your project](#publish-and-deploy-your-project-to-the-iis-site-folder).</span></span>

1. <span data-ttu-id="a6eba-158">In Gestione IIS selezionare il sito IIS sotto il **siti** nodo del **connessioni** nella barra laterale.</span><span class="sxs-lookup"><span data-stu-id="a6eba-158">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
1. <span data-ttu-id="a6eba-159">Fare doppio clic su **Authentication** nel **IIS** area.</span><span class="sxs-lookup"><span data-stu-id="a6eba-159">Double-click **Authentication** in the **IIS** area.</span></span>
1. <span data-ttu-id="a6eba-160">Selezionare **l'autenticazione anonima**.</span><span class="sxs-lookup"><span data-stu-id="a6eba-160">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="a6eba-161">Selezionare **disabilitare** nel **azioni** nella barra laterale.</span><span class="sxs-lookup"><span data-stu-id="a6eba-161">Select **Disable** in the **Actions** sidebar.</span></span>
1. <span data-ttu-id="a6eba-162">Selezionare **Windows autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="a6eba-162">Select **Windows Authentication**.</span></span> <span data-ttu-id="a6eba-163">Selezionare **abilitare** nel **azioni** nella barra laterale.</span><span class="sxs-lookup"><span data-stu-id="a6eba-163">Select **Enable** in the **Actions** sidebar.</span></span>

<span data-ttu-id="a6eba-164">Quando vengono eseguite queste operazioni, Gestione IIS modifica dell'app *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="a6eba-164">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="a6eba-165">Oggetto `<system.webServer><security><authentication>` nodo viene aggiunto con impostazioni aggiornate per `anonymousAuthentication` e `windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="a6eba-165">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

[!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

<span data-ttu-id="a6eba-166">Il `<system.webServer>` aggiunto alla sezione di *Web. config* file da Gestione IIS è di fuori dell'app `<location>` sezione aggiunte per .NET Core SDK quando la pubblicazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="a6eba-166">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="a6eba-167">Poiché la sezione viene aggiunta di fuori del `<location>` nodo, le impostazioni vengono ereditate da qualsiasi [App secondarie](xref:host-and-deploy/iis/index#sub-applications) all'app corrente.</span><span class="sxs-lookup"><span data-stu-id="a6eba-167">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="a6eba-168">Per impedire l'ereditarietà, spostare l'aggiunta `<security>` all'interno della sezione di `<location><system.webServer>` sezione in cui il SDK fornito.</span><span class="sxs-lookup"><span data-stu-id="a6eba-168">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the SDK provided.</span></span>

<span data-ttu-id="a6eba-169">Quando Gestione IIS consente di aggiungere la configurazione di IIS, questo interessa solo l'app *Web. config* file sul server.</span><span class="sxs-lookup"><span data-stu-id="a6eba-169">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="a6eba-170">Una distribuzione dell'app per le successive possa sovrascrivere le impostazioni nel server se la copia del server del *Web. config* viene sostituita da del progetto *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="a6eba-170">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="a6eba-171">Uso **entrambi** degli approcci seguenti per gestire le impostazioni:</span><span class="sxs-lookup"><span data-stu-id="a6eba-171">Use **either** of the following approaches to manage the settings:</span></span>

* <span data-ttu-id="a6eba-172">Utilizzare Gestione IIS per reimpostare le impostazioni nel *Web. config* file dopo che il file viene sovrascritto nella distribuzione.</span><span class="sxs-lookup"><span data-stu-id="a6eba-172">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
* <span data-ttu-id="a6eba-173">Aggiungere un *file Web. config* all'app in locale con le impostazioni.</span><span class="sxs-lookup"><span data-stu-id="a6eba-173">Add a *web.config file* to the app locally with the settings.</span></span> <span data-ttu-id="a6eba-174">Per altre informazioni, vedere la [configurazione lato sviluppo](#development-side-configuration-with-a-local-webconfig-file) sezione.</span><span class="sxs-lookup"><span data-stu-id="a6eba-174">For more information, see the [Development-side configuration](#development-side-configuration-with-a-local-webconfig-file) section.</span></span>

### <a name="publish-and-deploy-your-project-to-the-iis-site-folder"></a><span data-ttu-id="a6eba-175">Pubblicare e distribuire il progetto alla cartella del sito IIS</span><span class="sxs-lookup"><span data-stu-id="a6eba-175">Publish and deploy your project to the IIS site folder</span></span>

<span data-ttu-id="a6eba-176">Usa Visual Studio o .NET Core CLI, pubblicare e distribuire l'app per la cartella di destinazione.</span><span class="sxs-lookup"><span data-stu-id="a6eba-176">Using Visual Studio or the .NET Core CLI, publish and deploy the app to the destination folder.</span></span>

<span data-ttu-id="a6eba-177">Per altre informazioni sull'hosting con IIS, pubblicazione e distribuzione, vedere gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="a6eba-177">For more information on hosting with IIS, publishing, and deployment, see the following topics:</span></span>

* [<span data-ttu-id="a6eba-178">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="a6eba-178">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>

<span data-ttu-id="a6eba-179">Avviare l'app per verificare l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="a6eba-179">Launch the app to verify Windows Authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys"></a><span data-ttu-id="a6eba-180">Abilitare l'autenticazione di Windows con HTTP. sys</span><span class="sxs-lookup"><span data-stu-id="a6eba-180">Enable Windows Authentication with HTTP.sys</span></span>

<span data-ttu-id="a6eba-181">Anche se Kestrel non supporta l'autenticazione di Windows, è possibile usare [HTTP. sys](xref:fundamentals/servers/httpsys) per supportare gli scenari self-hosted in Windows.</span><span class="sxs-lookup"><span data-stu-id="a6eba-181">Although Kestrel doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="a6eba-182">L'esempio seguente configura l'host web dell'app per usare HTTP. sys con l'autenticazione di Windows:</span><span class="sxs-lookup"><span data-stu-id="a6eba-182">The following example configures the app's web host to use HTTP.sys with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Program.cs?highlight=9-14)]

> [!NOTE]
> <span data-ttu-id="a6eba-183">Per la delega all'autenticazione in modalità kernel, HTTP.sys usa il protocollo di autenticazione Kerberos.</span><span class="sxs-lookup"><span data-stu-id="a6eba-183">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="a6eba-184">L'autenticazione in modalità utente non è supportata con Kerberos e HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="a6eba-184">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="a6eba-185">È necessario usare l'account del computer per decrittografare il token/ticket Kerberos ottenuto da Active Directory e inoltrato dal client al server per autenticare l'utente.</span><span class="sxs-lookup"><span data-stu-id="a6eba-185">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="a6eba-186">Registrare il nome dell'entità servizio per l'host, non l'utente dell'app.</span><span class="sxs-lookup"><span data-stu-id="a6eba-186">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="a6eba-187">Http. sys non è supportata in Nano Server versione 1709 o successiva.</span><span class="sxs-lookup"><span data-stu-id="a6eba-187">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="a6eba-188">Per usare l'autenticazione di Windows e HTTP. sys con Nano Server, usare una [contenitore di Server Core (microsoft/windowsservercore)](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="a6eba-188">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="a6eba-189">Per altre informazioni su Server Core, vedere [qual è l'opzione di installazione Server Core in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="a6eba-189">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="work-with-windows-authentication"></a><span data-ttu-id="a6eba-190">Utilizzo con l'autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="a6eba-190">Work with Windows Authentication</span></span>

<span data-ttu-id="a6eba-191">Lo stato di configurazione dell'accesso anonimo determina il modo in cui il `[Authorize]` e `[AllowAnonymous]` attributi vengono usati nell'app.</span><span class="sxs-lookup"><span data-stu-id="a6eba-191">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="a6eba-192">Le due sezioni seguenti illustrano come gestire gli Stati non consentiti e consentito la configurazione dell'accesso anonimo.</span><span class="sxs-lookup"><span data-stu-id="a6eba-192">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="a6eba-193">Non consentire l'accesso anonimo</span><span class="sxs-lookup"><span data-stu-id="a6eba-193">Disallow anonymous access</span></span>

<span data-ttu-id="a6eba-194">Quando è abilitata l'autenticazione di Windows e accesso anonimo è disabilitato, il `[Authorize]` e `[AllowAnonymous]` attributi non hanno alcun effetto.</span><span class="sxs-lookup"><span data-stu-id="a6eba-194">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="a6eba-195">Se il sito IIS (o HTTP. sys) è configurato per non consentire l'accesso anonimo, la richiesta raggiunga mai l'app.</span><span class="sxs-lookup"><span data-stu-id="a6eba-195">If the IIS site (or HTTP.sys) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="a6eba-196">Per questo motivo, il `[AllowAnonymous]` attributo non è applicabile.</span><span class="sxs-lookup"><span data-stu-id="a6eba-196">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="a6eba-197">Consenti accesso anonimo</span><span class="sxs-lookup"><span data-stu-id="a6eba-197">Allow anonymous access</span></span>

<span data-ttu-id="a6eba-198">Quando sono abilitati sia l'autenticazione di Windows e l'accesso anonimo, usare il `[Authorize]` e `[AllowAnonymous]` attributi.</span><span class="sxs-lookup"><span data-stu-id="a6eba-198">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="a6eba-199">Il `[Authorize]` attributo consente di proteggere parti dell'app che realmente richiedono l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="a6eba-199">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows Authentication.</span></span> <span data-ttu-id="a6eba-200">Il `[AllowAnonymous]` esegue l'override dell'attributo `[Authorize]` utilizzo all'interno delle App che consentono l'accesso anonimo degli attributi.</span><span class="sxs-lookup"><span data-stu-id="a6eba-200">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="a6eba-201">Visualizzare [autorizzazione semplice](xref:security/authorization/simple) per informazioni dettagliate sull'utilizzo di attributi.</span><span class="sxs-lookup"><span data-stu-id="a6eba-201">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="a6eba-202">In ASP.NET Core 2.x, il `[Authorize]` attributo richiede una configurazione aggiuntiva nel *Startup.cs* per stimolare le richieste anonime per l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="a6eba-202">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows Authentication.</span></span> <span data-ttu-id="a6eba-203">La configurazione consigliata varia leggermente in base sul server web in uso.</span><span class="sxs-lookup"><span data-stu-id="a6eba-203">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="a6eba-204">Per impostazione predefinita, gli utenti che non dispongono di autorizzazione per accedere a una pagina vengono visualizzati una risposta HTTP 403 vuota.</span><span class="sxs-lookup"><span data-stu-id="a6eba-204">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="a6eba-205">Il [StatusCodePages middleware](xref:fundamentals/error-handling#configure-status-code-pages) può essere configurato per fornire agli utenti una migliore esperienza di "Accesso negato".</span><span class="sxs-lookup"><span data-stu-id="a6eba-205">The [StatusCodePages middleware](xref:fundamentals/error-handling#configure-status-code-pages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="a6eba-206">IIS</span><span class="sxs-lookup"><span data-stu-id="a6eba-206">IIS</span></span>

<span data-ttu-id="a6eba-207">Se si usa IIS, aggiungere il codice seguente il `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="a6eba-207">If using IIS, add the following to the `ConfigureServices` method:</span></span>

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="a6eba-208">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="a6eba-208">HTTP.sys</span></span>

<span data-ttu-id="a6eba-209">Se si Usa HTTP. sys, aggiungere il codice seguente il `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="a6eba-209">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="a6eba-210">Rappresentazione</span><span class="sxs-lookup"><span data-stu-id="a6eba-210">Impersonation</span></span>

<span data-ttu-id="a6eba-211">ASP.NET Core non implementa la rappresentazione.</span><span class="sxs-lookup"><span data-stu-id="a6eba-211">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="a6eba-212">Le app vengono eseguite con l'identità dell'app per tutte le richieste, usando l'identità del pool o un processo dell'app.</span><span class="sxs-lookup"><span data-stu-id="a6eba-212">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="a6eba-213">Se è necessario eseguire in modo esplicito un'azione per conto di un utente, usare [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in un [middleware inline terminal](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="a6eba-213">If you need to explicitly perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="a6eba-214">Eseguire una singola azione in questo contesto e quindi chiudere il contesto.</span><span class="sxs-lookup"><span data-stu-id="a6eba-214">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

<span data-ttu-id="a6eba-215">`RunImpersonated` non supporta operazioni asincrone e non deve essere usata per scenari complessi.</span><span class="sxs-lookup"><span data-stu-id="a6eba-215">`RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="a6eba-216">Ad esempio, di wrapping delle richieste intere o catene di middleware, non è supportato o consigliato.</span><span class="sxs-lookup"><span data-stu-id="a6eba-216">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

### <a name="claims-transformations"></a><span data-ttu-id="a6eba-217">Trasformazioni di attestazioni</span><span class="sxs-lookup"><span data-stu-id="a6eba-217">Claims transformations</span></span>

<span data-ttu-id="a6eba-218">Quando si ospitano con la modalità in-process IIS, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> non viene chiamato internamente per inizializzare un utente.</span><span class="sxs-lookup"><span data-stu-id="a6eba-218">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="a6eba-219">Pertanto, un <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementazione utilizzata per trasformare le attestazioni dopo ogni autenticazione non è attivato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="a6eba-219">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="a6eba-220">Per altre informazioni e un esempio di codice che attiva le trasformazioni di attestazioni per l'hosting in-process, vedere <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="a6eba-220">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>
