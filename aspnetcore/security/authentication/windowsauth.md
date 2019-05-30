---
title: Configurare l'autenticazione di Windows in ASP.NET Core
author: scottaddie
description: Informazioni su come configurare l'autenticazione di Windows in ASP.NET Core per HTTP. sys e IIS.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 05/29/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 9dfff5dcba409ddca7e05c771b864ab121e0ea85
ms.sourcegitcommit: 06c4f2910dd54ded25e1b8750e09c66578748bc9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2019
ms.locfileid: "66395934"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="2b1fd-103">Configurare l'autenticazione di Windows in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2b1fd-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="2b1fd-104">Dal [Scott Addie](https://twitter.com/Scott_Addie) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2b1fd-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2b1fd-105">[L'autenticazione di Windows](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) può essere configurato per le app ASP.NET Core ospitate con [IIS](xref:host-and-deploy/iis/index) oppure [HTTP. sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="2b1fd-105">[Windows Authentication](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="2b1fd-106">L'autenticazione di Windows si basa sul sistema operativo per autenticare gli utenti delle App ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-106">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="2b1fd-107">È possibile usare l'autenticazione di Windows quando il server in esecuzione in una rete aziendale usando le identità di dominio Active Directory o account di Windows per identificare gli utenti.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-107">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="2b1fd-108">L'autenticazione di Windows è più adatta agli ambienti intranet in cui gli utenti, le app client e server web appartengono allo stesso dominio di Windows.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-108">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

## <a name="launch-settings-debugger"></a><span data-ttu-id="2b1fd-109">Avviare le impostazioni (debugger)</span><span class="sxs-lookup"><span data-stu-id="2b1fd-109">Launch settings (debugger)</span></span>

<span data-ttu-id="2b1fd-110">Configurazione per le impostazioni di avvio interessa solo il *Properties/launchSettings.json* file e non configurare il server IIS o HTTP. sys per l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-110">Configuration for launch settings only affects the *Properties/launchSettings.json* file and doesn't configure the IIS or HTTP.sys server for Windows Authentication.</span></span> <span data-ttu-id="2b1fd-111">Configurazione del server viene illustrata nel [abilitare i servizi di autenticazione per IIS o HTTP. sys](#authentication-services-for-iis-or-httpsys) sezione.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-111">Configuration of the server is explained in the [Enable authentication services for IIS or HTTP.sys](#authentication-services-for-iis-or-httpsys) section.</span></span>

<span data-ttu-id="2b1fd-112">Il **applicazione Web** modello disponibile tramite la CLI di .NET Core o Visual Studio possono essere configurato per supportare l'autenticazione di Windows, che aggiorna il *Properties/launchSettings.json* file automaticamente.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-112">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication, which updates the *Properties/launchSettings.json* file automatically.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2b1fd-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2b1fd-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2b1fd-114">**nuovo progetto**</span><span class="sxs-lookup"><span data-stu-id="2b1fd-114">**New project**</span></span>

1. <span data-ttu-id="2b1fd-115">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-115">Create a new project.</span></span>
1. <span data-ttu-id="2b1fd-116">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-116">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="2b1fd-117">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-117">Select **Next**.</span></span>
1. <span data-ttu-id="2b1fd-118">Specificare un nome nella **nome progetto** campo.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-118">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="2b1fd-119">Verificare i **posizione** voce sia corretta o specificare un percorso per il progetto.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-119">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="2b1fd-120">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-120">Select **Create**.</span></span>
1. <span data-ttu-id="2b1fd-121">Selezionare **Change** sotto **autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-121">Select **Change** under **Authentication**.</span></span>
1. <span data-ttu-id="2b1fd-122">Nel **Modifica autenticazione** finestra, seleziona **l'autenticazione di Windows**.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-122">In the **Change Authentication** window, select **Windows Authentication**.</span></span> <span data-ttu-id="2b1fd-123">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-123">Select **OK**.</span></span>
1. <span data-ttu-id="2b1fd-124">Selezionare **Applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-124">Select **Web Application**.</span></span>
1. <span data-ttu-id="2b1fd-125">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-125">Select **Create**.</span></span>

<span data-ttu-id="2b1fd-126">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-126">Run the app.</span></span> <span data-ttu-id="2b1fd-127">Il nome utente viene visualizzato nell'interfaccia utente dell'app sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-127">The username appears in the rendered app's user interface.</span></span>

<span data-ttu-id="2b1fd-128">**Progetto esistente**</span><span class="sxs-lookup"><span data-stu-id="2b1fd-128">**Existing project**</span></span>

<span data-ttu-id="2b1fd-129">Le proprietà del progetto abilita l'autenticazione di Windows e disattivano l'autenticazione anonima:</span><span class="sxs-lookup"><span data-stu-id="2b1fd-129">The project's properties enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="2b1fd-130">Fare clic con il pulsante destro del mouse in **Esplora soluzioni** e scegliere **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-130">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="2b1fd-131">Selezionare la scheda **Debug**.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-131">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="2b1fd-132">Deselezionare la casella di controllo **abilitare l'autenticazione anonima**.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-132">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="2b1fd-133">Selezionare la casella di controllo **abilitare l'autenticazione di Windows**.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-133">Select the check box for **Enable Windows Authentication**.</span></span>
1. <span data-ttu-id="2b1fd-134">Salvare e chiudere la pagina delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-134">Save and close the property page.</span></span>

<span data-ttu-id="2b1fd-135">In alternativa, è possibile configurare le proprietà nel `iisSettings` nodo il *launchsettings. JSON* file:</span><span class="sxs-lookup"><span data-stu-id="2b1fd-135">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="2b1fd-136">Visual Studio Code/Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="2b1fd-136">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="2b1fd-137">**nuovo progetto**</span><span class="sxs-lookup"><span data-stu-id="2b1fd-137">**New project**</span></span>

<span data-ttu-id="2b1fd-138">Eseguire la [dotnet nuove](/dotnet/core/tools/dotnet-new) con il `webapp` argomento (App Web di ASP.NET Core) e `--auth Windows` passare:</span><span class="sxs-lookup"><span data-stu-id="2b1fd-138">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

<span data-ttu-id="2b1fd-139">**Progetto esistente**</span><span class="sxs-lookup"><span data-stu-id="2b1fd-139">**Existing project**</span></span>

<span data-ttu-id="2b1fd-140">Aggiornamento il `iisSettings` nodo il *launchsettings. JSON* file:</span><span class="sxs-lookup"><span data-stu-id="2b1fd-140">Update the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

<span data-ttu-id="2b1fd-141">Quando si modifica un progetto esistente, verificare che il file di progetto include un riferimento al pacchetto per il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) **oppure** il [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-141">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

## <a name="authentication-services-for-iis-or-httpsys"></a><span data-ttu-id="2b1fd-142">Servizi di autenticazione per IIS o HTTP. sys</span><span class="sxs-lookup"><span data-stu-id="2b1fd-142">Authentication services for IIS or HTTP.sys</span></span>

<span data-ttu-id="2b1fd-143">A seconda dello scenario di hosting, seguire le indicazioni fornite in **entrambe** le [IIS](#iis) sezione **oppure** [HTTP. sys](#httpsys) sezione.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-143">Depending on the hosting scenario, follow the guidance in **either** the [IIS](#iis) section **or** [HTTP.sys](#httpsys) section.</span></span>

### <a name="iis"></a><span data-ttu-id="2b1fd-144">IIS</span><span class="sxs-lookup"><span data-stu-id="2b1fd-144">IIS</span></span>

<span data-ttu-id="2b1fd-145">Aggiungere servizi di autenticazione richiamando <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> dello spazio dei nomi) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="2b1fd-145">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="2b1fd-146">IIS Usa il [modulo di ASP.NET Core](xref:host-and-deploy/aspnet-core-module) per ospitare App ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-146">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="2b1fd-147">L'autenticazione di Windows è configurato per IIS tramite il *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-147">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="2b1fd-148">Le sezioni seguenti mostrano come:</span><span class="sxs-lookup"><span data-stu-id="2b1fd-148">The following sections show how to:</span></span>

* <span data-ttu-id="2b1fd-149">Specificare una variabile locale *Web. config* file che attiva l'autenticazione di Windows nel server quando l'app viene distribuita.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-149">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="2b1fd-150">Usare Gestione IIS per configurare il *Web. config* file di un'app ASP.NET Core che è già stata distribuita nel server.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-150">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

<span data-ttu-id="2b1fd-151">Se non già stato fatto, abilitare IIS per ospitare App ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-151">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="2b1fd-152">Per altre informazioni, vedere <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-152">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="2b1fd-153">Abilitare il servizio ruolo IIS per l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-153">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="2b1fd-154">Per altre informazioni, vedere [abilitare l'autenticazione di Windows nei servizi di ruolo IIS (vedere il passaggio 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="2b1fd-154">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="2b1fd-155">[Middleware di integrazione IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) è configurato per autenticare automaticamente le richieste per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-155">[IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="2b1fd-156">Per altre informazioni, vedere [Host ASP.NET Core in Windows con IIS: Le opzioni di IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="2b1fd-156">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="2b1fd-157">Per impostazione predefinita, il modulo ASP.NET Core è configurato per inoltrare il token di autenticazione di Windows per l'app.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-157">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="2b1fd-158">Per altre informazioni, vedere [riferimento configurazione modulo ASP.NET Core: Attributi dell'elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="2b1fd-158">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

<span data-ttu-id="2b1fd-159">Uso **entrambi** degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="2b1fd-159">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="2b1fd-160">**Prima della pubblicazione e la distribuzione del progetto** aggiungere il codice seguente *Web. config* file alla radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="2b1fd-160">**Before publishing and deploying the project,** add the following *web.config* file to the project root:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  <span data-ttu-id="2b1fd-161">Quando il progetto viene pubblicato per .NET Core SDK (senza il `<IsTransformWebConfigDisabled>` impostata su `true` nel file di progetto), pubblicato *Web. config* file include il `<location><system.webServer><security><authentication>` sezione.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-161">When the project is published by the .NET Core SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="2b1fd-162">Per altre informazioni sul `<IsTransformWebConfigDisabled>` proprietà, vedere <xref:host-and-deploy/iis/index#webconfig-file>.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-162">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

* <span data-ttu-id="2b1fd-163">**Dopo la pubblicazione e la distribuzione del progetto,** eseguire la configurazione lato server con Gestione IIS:</span><span class="sxs-lookup"><span data-stu-id="2b1fd-163">**After publishing and deploying the project,** perform server-side configuration with the IIS Manager:</span></span>

  1. <span data-ttu-id="2b1fd-164">In Gestione IIS selezionare il sito IIS sotto il **siti** nodo del **connessioni** nella barra laterale.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-164">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
  1. <span data-ttu-id="2b1fd-165">Fare doppio clic su **Authentication** nel **IIS** area.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-165">Double-click **Authentication** in the **IIS** area.</span></span>
  1. <span data-ttu-id="2b1fd-166">Selezionare **l'autenticazione anonima**.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-166">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="2b1fd-167">Selezionare **disabilitare** nel **azioni** nella barra laterale.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-167">Select **Disable** in the **Actions** sidebar.</span></span>
  1. <span data-ttu-id="2b1fd-168">Selezionare **Windows autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-168">Select **Windows Authentication**.</span></span> <span data-ttu-id="2b1fd-169">Selezionare **abilitare** nel **azioni** nella barra laterale.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-169">Select **Enable** in the **Actions** sidebar.</span></span>

  <span data-ttu-id="2b1fd-170">Quando vengono eseguite queste operazioni, Gestione IIS modifica dell'app *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-170">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="2b1fd-171">Oggetto `<system.webServer><security><authentication>` nodo viene aggiunto con impostazioni aggiornate per `anonymousAuthentication` e `windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="2b1fd-171">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  <span data-ttu-id="2b1fd-172">Il `<system.webServer>` aggiunto alla sezione di *Web. config* file da Gestione IIS è di fuori dell'app `<location>` sezione aggiunte per .NET Core SDK quando la pubblicazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-172">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="2b1fd-173">Poiché la sezione viene aggiunta di fuori del `<location>` nodo, le impostazioni vengono ereditate da qualsiasi [App secondarie](xref:host-and-deploy/iis/index#sub-applications) all'app corrente.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-173">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="2b1fd-174">Per impedire l'ereditarietà, spostare l'aggiunta `<security>` all'interno della sezione di `<location><system.webServer>` sezione che .NET Core SDK fornito.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-174">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the .NET Core SDK provided.</span></span>

  <span data-ttu-id="2b1fd-175">Quando Gestione IIS consente di aggiungere la configurazione di IIS, questo interessa solo l'app *Web. config* file sul server.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-175">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="2b1fd-176">Una distribuzione dell'app per le successive possa sovrascrivere le impostazioni nel server se la copia del server del *Web. config* viene sostituita da del progetto *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-176">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="2b1fd-177">Uso **entrambi** degli approcci seguenti per gestire le impostazioni:</span><span class="sxs-lookup"><span data-stu-id="2b1fd-177">Use **either** of the following approaches to manage the settings:</span></span>

  * <span data-ttu-id="2b1fd-178">Utilizzare Gestione IIS per reimpostare le impostazioni nel *Web. config* file dopo che il file viene sovrascritto nella distribuzione.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-178">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
  * <span data-ttu-id="2b1fd-179">Aggiungere un *file Web. config* all'app in locale con le impostazioni.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-179">Add a *web.config file* to the app locally with the settings.</span></span>

### <a name="httpsys"></a><span data-ttu-id="2b1fd-180">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="2b1fd-180">HTTP.sys</span></span>

<span data-ttu-id="2b1fd-181">Sebbene [Kestrel](xref:fundamentals/servers/kestrel) non supporta l'autenticazione di Windows, è possibile usare [HTTP. sys](xref:fundamentals/servers/httpsys) per supportare gli scenari self-hosted in Windows.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-181">Although [Kestrel](xref:fundamentals/servers/kestrel) doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span>

<span data-ttu-id="2b1fd-182">Aggiungere servizi di autenticazione richiamando <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> dello spazio dei nomi) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="2b1fd-182">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

<span data-ttu-id="2b1fd-183">Configurare l'host web dell'app per usare HTTP. sys con l'autenticazione di Windows (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="2b1fd-183">Configure the app's web host to use HTTP.sys with Windows Authentication (*Program.cs*).</span></span> <span data-ttu-id="2b1fd-184"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> le novità di <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-184"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> is in the <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="2b1fd-185">Per la delega all'autenticazione in modalità kernel, HTTP.sys usa il protocollo di autenticazione Kerberos.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-185">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="2b1fd-186">L'autenticazione in modalità utente non è supportata con Kerberos e HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-186">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="2b1fd-187">È necessario usare l'account del computer per decrittografare il token/ticket Kerberos ottenuto da Active Directory e inoltrato dal client al server per autenticare l'utente.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-187">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="2b1fd-188">Registrare il nome dell'entità servizio per l'host, non l'utente dell'app.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-188">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="2b1fd-189">Http. sys non è supportata in Nano Server versione 1709 o successiva.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-189">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="2b1fd-190">Per usare l'autenticazione di Windows e HTTP. sys con Nano Server, usare una [contenitore di Server Core (microsoft/windowsservercore)](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="2b1fd-190">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="2b1fd-191">Per altre informazioni su Server Core, vedere [qual è l'opzione di installazione Server Core in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="2b1fd-191">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="authorize-users"></a><span data-ttu-id="2b1fd-192">Autorizzare gli utenti</span><span class="sxs-lookup"><span data-stu-id="2b1fd-192">Authorize users</span></span>

<span data-ttu-id="2b1fd-193">Lo stato di configurazione dell'accesso anonimo determina il modo in cui il `[Authorize]` e `[AllowAnonymous]` attributi vengono usati nell'app.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-193">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="2b1fd-194">Le due sezioni seguenti illustrano come gestire gli Stati non consentiti e consentito la configurazione dell'accesso anonimo.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-194">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="2b1fd-195">Non consentire l'accesso anonimo</span><span class="sxs-lookup"><span data-stu-id="2b1fd-195">Disallow anonymous access</span></span>

<span data-ttu-id="2b1fd-196">Quando è abilitata l'autenticazione di Windows e accesso anonimo è disabilitato, il `[Authorize]` e `[AllowAnonymous]` attributi non hanno alcun effetto.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-196">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="2b1fd-197">Se un sito IIS è configurato per non consentire l'accesso anonimo, la richiesta raggiunga mai l'app.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-197">If an IIS site is configured to disallow anonymous access, the request never reaches the app.</span></span> <span data-ttu-id="2b1fd-198">Per questo motivo, il `[AllowAnonymous]` attributo non è applicabile.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-198">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="2b1fd-199">Consenti accesso anonimo</span><span class="sxs-lookup"><span data-stu-id="2b1fd-199">Allow anonymous access</span></span>

<span data-ttu-id="2b1fd-200">Quando sono abilitati sia l'autenticazione di Windows e l'accesso anonimo, usare il `[Authorize]` e `[AllowAnonymous]` attributi.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-200">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="2b1fd-201">Il `[Authorize]` attributo consente di proteggere gli endpoint dell'app che richiedono l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-201">The `[Authorize]` attribute allows you to secure endpoints of the app which require authentication.</span></span> <span data-ttu-id="2b1fd-202">Il `[AllowAnonymous]` esegue l'override dell'attributo di `[Authorize]` attributo nelle App che consente l'accesso anonimo.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-202">The `[AllowAnonymous]` attribute overrides the `[Authorize]` attribute in apps that allow anonymous access.</span></span> <span data-ttu-id="2b1fd-203">Per informazioni dettagliate sull'utilizzo di attributi, vedere <xref:security/authorization/simple>.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-203">For attribute usage details, see <xref:security/authorization/simple>.</span></span>

> [!NOTE]
> <span data-ttu-id="2b1fd-204">Per impostazione predefinita, gli utenti che non dispongono di autorizzazione per accedere a una pagina vengono visualizzati una risposta HTTP 403 vuota.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-204">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="2b1fd-205">Il [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) può essere configurato per fornire agli utenti una migliore esperienza di "Accesso negato".</span><span class="sxs-lookup"><span data-stu-id="2b1fd-205">The [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) can be configured to provide users with a better "Access Denied" experience.</span></span>

## <a name="impersonation"></a><span data-ttu-id="2b1fd-206">Rappresentazione</span><span class="sxs-lookup"><span data-stu-id="2b1fd-206">Impersonation</span></span>

<span data-ttu-id="2b1fd-207">ASP.NET Core non implementa la rappresentazione.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-207">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="2b1fd-208">Le app vengono eseguite con l'identità dell'app per tutte le richieste, usando l'identità del pool o un processo dell'app.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-208">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="2b1fd-209">Se l'app deve eseguire un'azione per conto di un utente, usare [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in un [middleware inline terminal](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-209">If the app should perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="2b1fd-210">Eseguire una singola azione in questo contesto e quindi chiudere il contesto.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-210">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

<span data-ttu-id="2b1fd-211">`RunImpersonated` non supporta operazioni asincrone e non deve essere usata per scenari complessi.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-211">`RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="2b1fd-212">Ad esempio, di wrapping delle richieste intere o catene di middleware, non è supportato o consigliato.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-212">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

## <a name="claims-transformations"></a><span data-ttu-id="2b1fd-213">Trasformazioni di attestazioni</span><span class="sxs-lookup"><span data-stu-id="2b1fd-213">Claims transformations</span></span>

<span data-ttu-id="2b1fd-214">Quando si ospitano con la modalità in-process IIS, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> non viene chiamato internamente per inizializzare un utente.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-214">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="2b1fd-215">Pertanto, un'implementazione di <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> usate per trasformare le attestazioni dopo ogni autenticazione non viene attivata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-215">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="2b1fd-216">Per altre informazioni e un esempio di codice che attiva le trasformazioni di attestazioni per l'hosting in-process, vedere <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="2b1fd-216">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2b1fd-217">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="2b1fd-217">Additional resources</span></span>

* [<span data-ttu-id="2b1fd-218">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="2b1fd-218">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
