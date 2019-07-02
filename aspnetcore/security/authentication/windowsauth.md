---
title: Configurare l'autenticazione di Windows in ASP.NET Core
author: scottaddie
description: Informazioni su come configurare l'autenticazione di Windows in ASP.NET Core per HTTP. sys e IIS.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 07/01/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 30f1f554a29412ed6b84115d457d2da1aba91c17
ms.sourcegitcommit: eb3e51d58dd713eefc242148f45bd9486be3a78a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/02/2019
ms.locfileid: "67500500"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="a3c58-103">Configurare l'autenticazione di Windows in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a3c58-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="a3c58-104">Dal [Scott Addie](https://twitter.com/Scott_Addie) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a3c58-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a3c58-105">L'autenticazione di Windows (noto anche come autenticazione Negotiate, Kerberos o NTLM) può essere configurato per le app ASP.NET Core ospitate con [IIS](xref:host-and-deploy/iis/index), [Kestrel](xref:fundamentals/servers/kestrel), o [HTTP. sys](xref:fundamentals/servers/httpsys) .</span><span class="sxs-lookup"><span data-stu-id="a3c58-105">Windows Authentication (also known as Negotiate, Kerberos, or NTLM authentication) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index), [Kestrel](xref:fundamentals/servers/kestrel), or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a3c58-106">L'autenticazione di Windows (noto anche come autenticazione Negotiate, Kerberos o NTLM) può essere configurato per le app ASP.NET Core ospitate con [IIS](xref:host-and-deploy/iis/index) oppure [HTTP. sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="a3c58-106">Windows Authentication (also known as Negotiate, Kerberos, or NTLM authentication) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

::: moniker-end

<span data-ttu-id="a3c58-107">L'autenticazione di Windows si basa sul sistema operativo per autenticare gli utenti delle App ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a3c58-107">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="a3c58-108">È possibile usare l'autenticazione di Windows quando il server in esecuzione in una rete aziendale usando le identità di dominio Active Directory o account di Windows per identificare gli utenti.</span><span class="sxs-lookup"><span data-stu-id="a3c58-108">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="a3c58-109">L'autenticazione di Windows è più adatta agli ambienti intranet in cui gli utenti, le app client e server web appartengono allo stesso dominio di Windows.</span><span class="sxs-lookup"><span data-stu-id="a3c58-109">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

> [!NOTE]
> <span data-ttu-id="a3c58-110">L'autenticazione di Windows non è supportata con HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="a3c58-110">Windows Authentication isn't supported with HTTP/2.</span></span> <span data-ttu-id="a3c58-111">Problemi di autenticazione possono essere inviati nelle risposte HTTP/2, ma il client deve effettuare il downgrade al protocollo HTTP/1.1 prima l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="a3c58-111">Authentication challenges can be sent on HTTP/2 responses, but the client must downgrade to HTTP/1.1 before authenticating.</span></span>

## <a name="iisiis-express"></a><span data-ttu-id="a3c58-112">IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="a3c58-112">IIS/IIS Express</span></span>

<span data-ttu-id="a3c58-113">Aggiungere servizi di autenticazione richiamando <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> dello spazio dei nomi) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a3c58-113">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

### <a name="launch-settings-debugger"></a><span data-ttu-id="a3c58-114">Avviare le impostazioni (debugger)</span><span class="sxs-lookup"><span data-stu-id="a3c58-114">Launch settings (debugger)</span></span>

<span data-ttu-id="a3c58-115">Configurazione per le impostazioni di avvio interessa solo il *Properties/launchSettings.json* per IIS Express e non configurare IIS per l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="a3c58-115">Configuration for launch settings only affects the *Properties/launchSettings.json* file for IIS Express and doesn't configure IIS for Windows Authentication.</span></span> <span data-ttu-id="a3c58-116">Configurazione del server viene illustrata nel [IIS](#iis) sezione.</span><span class="sxs-lookup"><span data-stu-id="a3c58-116">Server configuration is explained in the [IIS](#iis) section.</span></span>

<span data-ttu-id="a3c58-117">Il **applicazione Web** modello disponibile tramite la CLI di .NET Core o Visual Studio possono essere configurato per supportare l'autenticazione di Windows, che aggiorna il *Properties/launchSettings.json* file automaticamente.</span><span class="sxs-lookup"><span data-stu-id="a3c58-117">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication, which updates the *Properties/launchSettings.json* file automatically.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a3c58-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a3c58-118">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a3c58-119">**nuovo progetto**</span><span class="sxs-lookup"><span data-stu-id="a3c58-119">**New project**</span></span>

1. <span data-ttu-id="a3c58-120">Creare un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="a3c58-120">Create a new project.</span></span>
1. <span data-ttu-id="a3c58-121">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a3c58-121">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="a3c58-122">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="a3c58-122">Select **Next**.</span></span>
1. <span data-ttu-id="a3c58-123">Specificare un nome nella **nome progetto** campo.</span><span class="sxs-lookup"><span data-stu-id="a3c58-123">Provide a name in the **Project name** field.</span></span> <span data-ttu-id="a3c58-124">Verificare i **posizione** voce sia corretta o specificare un percorso per il progetto.</span><span class="sxs-lookup"><span data-stu-id="a3c58-124">Confirm the **Location** entry is correct or provide a location for the project.</span></span> <span data-ttu-id="a3c58-125">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="a3c58-125">Select **Create**.</span></span>
1. <span data-ttu-id="a3c58-126">Selezionare **Change** sotto **autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="a3c58-126">Select **Change** under **Authentication**.</span></span>
1. <span data-ttu-id="a3c58-127">Nel **Modifica autenticazione** finestra, seleziona **l'autenticazione di Windows**.</span><span class="sxs-lookup"><span data-stu-id="a3c58-127">In the **Change Authentication** window, select **Windows Authentication**.</span></span> <span data-ttu-id="a3c58-128">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="a3c58-128">Select **OK**.</span></span>
1. <span data-ttu-id="a3c58-129">Selezionare **Applicazione Web**.</span><span class="sxs-lookup"><span data-stu-id="a3c58-129">Select **Web Application**.</span></span>
1. <span data-ttu-id="a3c58-130">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="a3c58-130">Select **Create**.</span></span>

<span data-ttu-id="a3c58-131">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="a3c58-131">Run the app.</span></span> <span data-ttu-id="a3c58-132">Il nome utente viene visualizzato nell'interfaccia utente dell'app sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="a3c58-132">The username appears in the rendered app's user interface.</span></span>

<span data-ttu-id="a3c58-133">**Progetto esistente**</span><span class="sxs-lookup"><span data-stu-id="a3c58-133">**Existing project**</span></span>

<span data-ttu-id="a3c58-134">Le proprietà del progetto abilita l'autenticazione di Windows e disattivano l'autenticazione anonima:</span><span class="sxs-lookup"><span data-stu-id="a3c58-134">The project's properties enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="a3c58-135">Fare clic con il pulsante destro del mouse in **Esplora soluzioni** e scegliere **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="a3c58-135">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="a3c58-136">Selezionare la scheda **Debug**.</span><span class="sxs-lookup"><span data-stu-id="a3c58-136">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="a3c58-137">Deselezionare la casella di controllo **abilitare l'autenticazione anonima**.</span><span class="sxs-lookup"><span data-stu-id="a3c58-137">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="a3c58-138">Selezionare la casella di controllo **abilitare l'autenticazione di Windows**.</span><span class="sxs-lookup"><span data-stu-id="a3c58-138">Select the check box for **Enable Windows Authentication**.</span></span>
1. <span data-ttu-id="a3c58-139">Salvare e chiudere la pagina delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="a3c58-139">Save and close the property page.</span></span>

<span data-ttu-id="a3c58-140">In alternativa, è possibile configurare le proprietà nel `iisSettings` nodo il *launchsettings. JSON* file:</span><span class="sxs-lookup"><span data-stu-id="a3c58-140">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="a3c58-141">Visual Studio Code/Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="a3c58-141">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="a3c58-142">**nuovo progetto**</span><span class="sxs-lookup"><span data-stu-id="a3c58-142">**New project**</span></span>

<span data-ttu-id="a3c58-143">Eseguire la [dotnet nuove](/dotnet/core/tools/dotnet-new) con il `webapp` argomento (App Web di ASP.NET Core) e `--auth Windows` passare:</span><span class="sxs-lookup"><span data-stu-id="a3c58-143">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

<span data-ttu-id="a3c58-144">**Progetto esistente**</span><span class="sxs-lookup"><span data-stu-id="a3c58-144">**Existing project**</span></span>

<span data-ttu-id="a3c58-145">Aggiornamento il `iisSettings` nodo il *launchsettings. JSON* file:</span><span class="sxs-lookup"><span data-stu-id="a3c58-145">Update the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

<span data-ttu-id="a3c58-146">Quando si modifica un progetto esistente, verificare che il file di progetto include un riferimento al pacchetto per il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) **oppure** il [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="a3c58-146">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

### <a name="iis"></a><span data-ttu-id="a3c58-147">IIS</span><span class="sxs-lookup"><span data-stu-id="a3c58-147">IIS</span></span>

<span data-ttu-id="a3c58-148">IIS Usa il [modulo di ASP.NET Core](xref:host-and-deploy/aspnet-core-module) per ospitare App ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a3c58-148">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="a3c58-149">L'autenticazione di Windows è configurato per IIS tramite il *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="a3c58-149">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="a3c58-150">Le sezioni seguenti mostrano come:</span><span class="sxs-lookup"><span data-stu-id="a3c58-150">The following sections show how to:</span></span>

* <span data-ttu-id="a3c58-151">Specificare una variabile locale *Web. config* file che attiva l'autenticazione di Windows nel server quando l'app viene distribuita.</span><span class="sxs-lookup"><span data-stu-id="a3c58-151">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="a3c58-152">Usare Gestione IIS per configurare il *Web. config* file di un'app ASP.NET Core che è già stata distribuita nel server.</span><span class="sxs-lookup"><span data-stu-id="a3c58-152">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

<span data-ttu-id="a3c58-153">Se non già stato fatto, abilitare IIS per ospitare App ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a3c58-153">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="a3c58-154">Per altre informazioni, vedere <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="a3c58-154">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="a3c58-155">Abilitare il servizio ruolo IIS per l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="a3c58-155">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="a3c58-156">Per altre informazioni, vedere [abilitare l'autenticazione di Windows nei servizi di ruolo IIS (vedere il passaggio 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="a3c58-156">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="a3c58-157">[Middleware di integrazione IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) è configurato per autenticare automaticamente le richieste per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="a3c58-157">[IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="a3c58-158">Per altre informazioni, vedere [Host ASP.NET Core in Windows con IIS: Le opzioni di IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="a3c58-158">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="a3c58-159">Per impostazione predefinita, il modulo ASP.NET Core è configurato per inoltrare il token di autenticazione di Windows per l'app.</span><span class="sxs-lookup"><span data-stu-id="a3c58-159">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="a3c58-160">Per altre informazioni, vedere [riferimento configurazione modulo ASP.NET Core: Attributi dell'elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="a3c58-160">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

<span data-ttu-id="a3c58-161">Uso **entrambi** degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="a3c58-161">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="a3c58-162">**Prima della pubblicazione e la distribuzione del progetto** aggiungere il codice seguente *Web. config* file alla radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="a3c58-162">**Before publishing and deploying the project,** add the following *web.config* file to the project root:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  <span data-ttu-id="a3c58-163">Quando il progetto viene pubblicato per .NET Core SDK (senza il `<IsTransformWebConfigDisabled>` impostata su `true` nel file di progetto), pubblicato *Web. config* file include il `<location><system.webServer><security><authentication>` sezione.</span><span class="sxs-lookup"><span data-stu-id="a3c58-163">When the project is published by the .NET Core SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="a3c58-164">Per altre informazioni sul `<IsTransformWebConfigDisabled>` proprietà, vedere <xref:host-and-deploy/iis/index#webconfig-file>.</span><span class="sxs-lookup"><span data-stu-id="a3c58-164">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

* <span data-ttu-id="a3c58-165">**Dopo la pubblicazione e la distribuzione del progetto,** eseguire la configurazione lato server con Gestione IIS:</span><span class="sxs-lookup"><span data-stu-id="a3c58-165">**After publishing and deploying the project,** perform server-side configuration with the IIS Manager:</span></span>

  1. <span data-ttu-id="a3c58-166">In Gestione IIS selezionare il sito IIS sotto il **siti** nodo del **connessioni** nella barra laterale.</span><span class="sxs-lookup"><span data-stu-id="a3c58-166">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
  1. <span data-ttu-id="a3c58-167">Fare doppio clic su **Authentication** nel **IIS** area.</span><span class="sxs-lookup"><span data-stu-id="a3c58-167">Double-click **Authentication** in the **IIS** area.</span></span>
  1. <span data-ttu-id="a3c58-168">Selezionare **l'autenticazione anonima**.</span><span class="sxs-lookup"><span data-stu-id="a3c58-168">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="a3c58-169">Selezionare **disabilitare** nel **azioni** nella barra laterale.</span><span class="sxs-lookup"><span data-stu-id="a3c58-169">Select **Disable** in the **Actions** sidebar.</span></span>
  1. <span data-ttu-id="a3c58-170">Selezionare **Windows autenticazione**.</span><span class="sxs-lookup"><span data-stu-id="a3c58-170">Select **Windows Authentication**.</span></span> <span data-ttu-id="a3c58-171">Selezionare **abilitare** nel **azioni** nella barra laterale.</span><span class="sxs-lookup"><span data-stu-id="a3c58-171">Select **Enable** in the **Actions** sidebar.</span></span>

  <span data-ttu-id="a3c58-172">Quando vengono eseguite queste operazioni, Gestione IIS modifica dell'app *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="a3c58-172">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="a3c58-173">Oggetto `<system.webServer><security><authentication>` nodo viene aggiunto con impostazioni aggiornate per `anonymousAuthentication` e `windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="a3c58-173">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  <span data-ttu-id="a3c58-174">Il `<system.webServer>` aggiunto alla sezione di *Web. config* file da Gestione IIS è di fuori dell'app `<location>` sezione aggiunte per .NET Core SDK quando la pubblicazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="a3c58-174">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="a3c58-175">Poiché la sezione viene aggiunta di fuori del `<location>` nodo, le impostazioni vengono ereditate da qualsiasi [App secondarie](xref:host-and-deploy/iis/index#sub-applications) all'app corrente.</span><span class="sxs-lookup"><span data-stu-id="a3c58-175">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="a3c58-176">Per impedire l'ereditarietà, spostare l'aggiunta `<security>` all'interno della sezione di `<location><system.webServer>` sezione che .NET Core SDK fornito.</span><span class="sxs-lookup"><span data-stu-id="a3c58-176">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the .NET Core SDK provided.</span></span>

  <span data-ttu-id="a3c58-177">Quando Gestione IIS consente di aggiungere la configurazione di IIS, questo interessa solo l'app *Web. config* file sul server.</span><span class="sxs-lookup"><span data-stu-id="a3c58-177">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="a3c58-178">Una distribuzione dell'app per le successive possa sovrascrivere le impostazioni nel server se la copia del server del *Web. config* viene sostituita da del progetto *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="a3c58-178">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="a3c58-179">Uso **entrambi** degli approcci seguenti per gestire le impostazioni:</span><span class="sxs-lookup"><span data-stu-id="a3c58-179">Use **either** of the following approaches to manage the settings:</span></span>

  * <span data-ttu-id="a3c58-180">Utilizzare Gestione IIS per reimpostare le impostazioni nel *Web. config* file dopo che il file viene sovrascritto nella distribuzione.</span><span class="sxs-lookup"><span data-stu-id="a3c58-180">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
  * <span data-ttu-id="a3c58-181">Aggiungere un *file Web. config* all'app in locale con le impostazioni.</span><span class="sxs-lookup"><span data-stu-id="a3c58-181">Add a *web.config file* to the app locally with the settings.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="kestrel"></a><span data-ttu-id="a3c58-182">Kestrel</span><span class="sxs-lookup"><span data-stu-id="a3c58-182">Kestrel</span></span>

 <span data-ttu-id="a3c58-183">Il [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) pacchetto NuGet può essere utilizzato con [Kestrel](xref:fundamentals/servers/kestrel) per supportare l'autenticazione di Windows utilizzando Negotiate, Kerberos e NTLM su Windows, Linux e macOS.</span><span class="sxs-lookup"><span data-stu-id="a3c58-183">The [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) NuGet package can be used with [Kestrel](xref:fundamentals/servers/kestrel) to support Windows Authentication using Negotiate, Kerberos, and NTLM on Windows, Linux, and macOS.</span></span>

> [!WARNING]
> <span data-ttu-id="a3c58-184">Credenziali possono essere reso persistente tra le richieste su una connessione.</span><span class="sxs-lookup"><span data-stu-id="a3c58-184">Credentials can be persisted across requests on a connection.</span></span> <span data-ttu-id="a3c58-185">*Negozia l'autenticazione non deve essere usata con i proxy, a meno che il proxy mantiene un'affinità di connessione 1:1 (una connessione permanente) con Kestrel.*</span><span class="sxs-lookup"><span data-stu-id="a3c58-185">*Negotiate authentication must not be used with proxies unless the proxy maintains a 1:1 connection affinity (a persistent connection) with Kestrel.*</span></span>

> [!NOTE]
> <span data-ttu-id="a3c58-186">Il gestore Negotiate rileva se il server sottostante supporta l'autenticazione di Windows in modo nativo e se è abilitato.</span><span class="sxs-lookup"><span data-stu-id="a3c58-186">The Negotiate handler detects if the underlying server supports Windows Authentication natively and if it's enabled.</span></span> <span data-ttu-id="a3c58-187">Se il server supporta l'autenticazione di Windows, ma è disabilitato, viene generato un errore che chiede di consentire l'implementazione del server.</span><span class="sxs-lookup"><span data-stu-id="a3c58-187">If the server supports Windows Authentication but it's disabled, an error is thrown asking you to enable the server implementation.</span></span> <span data-ttu-id="a3c58-188">Quando è abilitata l'autenticazione di Windows nel server, il gestore Negotiate inoltra in modo trasparente ad esso.</span><span class="sxs-lookup"><span data-stu-id="a3c58-188">When Windows Authentication is enabled in the server, the Negotiate handler transparently forwards to it.</span></span>

 <span data-ttu-id="a3c58-189">Aggiungere servizi di autenticazione richiamando <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (`Microsoft.AspNetCore.Authentication.Negotiate` dello spazio dei nomi) e `AddNegotitate` (`Microsoft.AspNetCore.Authentication.Negotiate` dello spazio dei nomi) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a3c58-189">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (`Microsoft.AspNetCore.Authentication.Negotiate` namespace) and `AddNegotitate` (`Microsoft.AspNetCore.Authentication.Negotiate` namespace) in `Startup.ConfigureServices`:</span></span>

 ```csharp
services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
    .AddNegotiate();
```

<span data-ttu-id="a3c58-190">Aggiungere il Middleware di autenticazione chiamando <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> in `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="a3c58-190">Add Authentication Middleware by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> in `Startup.Configure`:</span></span>

 ```csharp
app.UseAuthentication();

app.UseMvc();
```

<span data-ttu-id="a3c58-191">Per altre informazioni sul middleware, vedere <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="a3c58-191">For more information on middleware, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="a3c58-192">Sono consentite richieste anonime.</span><span class="sxs-lookup"><span data-stu-id="a3c58-192">Anonymous requests are allowed.</span></span> <span data-ttu-id="a3c58-193">Uso [autorizzazione ASP.NET Core](xref:security/authorization/introduction) per stimolare le richieste anonime per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="a3c58-193">Use [ASP.NET Core Authorization](xref:security/authorization/introduction) to challenge anonymous requests for authentication.</span></span>

### <a name="windows-environment-configuration"></a><span data-ttu-id="a3c58-194">Configurazione dell'ambiente di Windows</span><span class="sxs-lookup"><span data-stu-id="a3c58-194">Windows environment configuration</span></span>

<span data-ttu-id="a3c58-195">Il [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) componente esegue l'autenticazione in modalità utente.</span><span class="sxs-lookup"><span data-stu-id="a3c58-195">The [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) component performs User Mode authentication.</span></span> <span data-ttu-id="a3c58-196">Nomi dell'entità servizio (SPN) deve essere aggiunto all'account utente che esegue il servizio, non l'account del computer.</span><span class="sxs-lookup"><span data-stu-id="a3c58-196">Service Principal Names (SPNs) must be added to the user account running the service, not the machine account.</span></span> <span data-ttu-id="a3c58-197">Eseguire `setspn -S HTTP/mysrevername.mydomain.com myuser` in una shell dei comandi amministrativo.</span><span class="sxs-lookup"><span data-stu-id="a3c58-197">Execute `setspn -S HTTP/mysrevername.mydomain.com myuser` in an administrative command shell.</span></span>

### <a name="linux-and-macos-environment-configuration"></a><span data-ttu-id="a3c58-198">Configurazione dell'ambiente di Linux e macOS</span><span class="sxs-lookup"><span data-stu-id="a3c58-198">Linux and macOS environment configuration</span></span>

<span data-ttu-id="a3c58-199">Le istruzioni per la partecipazione a un computer Linux o macOS a un dominio di Windows sono disponibili nel [connessione di Studio di Azure Data a SQL Server utilizzando l'autenticazione di Windows - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) articolo.</span><span class="sxs-lookup"><span data-stu-id="a3c58-199">Instructions for joining a Linux or macOS machine to a Windows domain are available in the [Connect Azure Data Studio to your SQL Server using Windows authentication - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) article.</span></span> <span data-ttu-id="a3c58-200">Le istruzioni di creare un account computer per il computer Linux nel dominio.</span><span class="sxs-lookup"><span data-stu-id="a3c58-200">The instructions create a machine account for the Linux machine on the domain.</span></span> <span data-ttu-id="a3c58-201">I nomi SPN devono essere aggiunto a tale account del computer.</span><span class="sxs-lookup"><span data-stu-id="a3c58-201">SPNs must be added to that machine account.</span></span>

> [!NOTE]
> <span data-ttu-id="a3c58-202">Quando si seguono le istruzioni fornite nel [connessione di Studio di Azure Data a SQL Server utilizzando l'autenticazione di Windows - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) articolo, sostituire `python-software-properties` con `python3-software-properties` se necessario.</span><span class="sxs-lookup"><span data-stu-id="a3c58-202">When following the guidance in the [Connect Azure Data Studio to your SQL Server using Windows authentication - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) article, replace `python-software-properties` with `python3-software-properties` if needed.</span></span>

<span data-ttu-id="a3c58-203">Quando il computer Linux o macOS è aggiunto al dominio, sono necessari passaggi aggiuntivi per fornire una [file keytab](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) con i nomi SPN:</span><span class="sxs-lookup"><span data-stu-id="a3c58-203">Once the Linux or macOS machine is joined to the domain, additional steps are required to provide a [keytab file](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) with the SPNs:</span></span>

* <span data-ttu-id="a3c58-204">Nel controller di dominio, aggiungere nuovi nomi SPN del servizio web per l'account del computer:</span><span class="sxs-lookup"><span data-stu-id="a3c58-204">On the domain controller, add new web service SPNs to the machine account:</span></span>
  * `setspn -S HTTP/mywebservice.mydomain.com mymachine`
  * `setspn -S HTTP/mywebservice@MYDOMAIN.COM mymachine`
* <span data-ttu-id="a3c58-205">Uso [molte](/windows-server/administration/windows-commands/ktpass) per generare un file keytab:</span><span class="sxs-lookup"><span data-stu-id="a3c58-205">Use [ktpass](/windows-server/administration/windows-commands/ktpass) to generate a keytab file:</span></span>
  * `ktpass -princ HTTP/mywebservice.mydomain.com@MYDOMAIN.COM -pass myKeyTabFilePassword -mapuser MYDOMAIN\mymachine$ -pType KRB5_NT_PRINCIPAL -out c:\temp\mymachine.HTTP.keytab -crypto AES256-SHA1`
  * <span data-ttu-id="a3c58-206">Alcuni campi devono essere specificate in lettere maiuscole come indicato.</span><span class="sxs-lookup"><span data-stu-id="a3c58-206">Some fields must be specified in uppercase as indicated.</span></span>
* <span data-ttu-id="a3c58-207">Copiare il file keytab al computer Linux o macOS.</span><span class="sxs-lookup"><span data-stu-id="a3c58-207">Copy the keytab file to the Linux or macOS machine.</span></span>
* <span data-ttu-id="a3c58-208">Selezionare il file keytab tramite una variabile di ambiente: `export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`</span><span class="sxs-lookup"><span data-stu-id="a3c58-208">Select the keytab file via an environment variable: `export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`</span></span>
* <span data-ttu-id="a3c58-209">Richiamare `klist` per mostrare i nomi SPN attualmente disponibili per l'uso.</span><span class="sxs-lookup"><span data-stu-id="a3c58-209">Invoke `klist` to show the SPNs currently available for use.</span></span>

> [!NOTE]
> <span data-ttu-id="a3c58-210">File keytab contiene le credenziali di accesso di dominio e debba essere adeguatamente protette.</span><span class="sxs-lookup"><span data-stu-id="a3c58-210">A keytab file contains domain access credentials and must be protected accordingly.</span></span>

::: moniker-end

## <a name="httpsys"></a><span data-ttu-id="a3c58-211">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="a3c58-211">HTTP.sys</span></span>

<span data-ttu-id="a3c58-212">[Http. sys](xref:fundamentals/servers/httpsys) supporta l'autenticazione in modalità Kernel Windows utilizzando Negotiate, NTLM o l'autenticazione di base.</span><span class="sxs-lookup"><span data-stu-id="a3c58-212">[HTTP.sys](xref:fundamentals/servers/httpsys) supports Kernel Mode Windows Authentication using Negotiate, NTLM, or Basic authentication.</span></span>

<span data-ttu-id="a3c58-213">Aggiungere servizi di autenticazione richiamando <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> dello spazio dei nomi) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a3c58-213">Add authentication services by invoking <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

<span data-ttu-id="a3c58-214">Configurare l'host web dell'app per usare HTTP. sys con l'autenticazione di Windows (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="a3c58-214">Configure the app's web host to use HTTP.sys with Windows Authentication (*Program.cs*).</span></span> <span data-ttu-id="a3c58-215"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> le novità di <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="a3c58-215"><xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> is in the <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> namespace.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="a3c58-216">Per la delega all'autenticazione in modalità kernel, HTTP.sys usa il protocollo di autenticazione Kerberos.</span><span class="sxs-lookup"><span data-stu-id="a3c58-216">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="a3c58-217">L'autenticazione in modalità utente non è supportata con Kerberos e HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="a3c58-217">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="a3c58-218">È necessario usare l'account del computer per decrittografare il token/ticket Kerberos ottenuto da Active Directory e inoltrato dal client al server per autenticare l'utente.</span><span class="sxs-lookup"><span data-stu-id="a3c58-218">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="a3c58-219">Registrare il nome dell'entità servizio per l'host, non l'utente dell'app.</span><span class="sxs-lookup"><span data-stu-id="a3c58-219">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="a3c58-220">Http. sys non è supportata in Nano Server versione 1709 o successiva.</span><span class="sxs-lookup"><span data-stu-id="a3c58-220">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="a3c58-221">Per usare l'autenticazione di Windows e HTTP. sys con Nano Server, usare una [contenitore di Server Core (microsoft/windowsservercore)](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="a3c58-221">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="a3c58-222">Per altre informazioni su Server Core, vedere [qual è l'opzione di installazione Server Core in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="a3c58-222">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="authorize-users"></a><span data-ttu-id="a3c58-223">Autorizzare gli utenti</span><span class="sxs-lookup"><span data-stu-id="a3c58-223">Authorize users</span></span>

<span data-ttu-id="a3c58-224">Lo stato di configurazione dell'accesso anonimo determina il modo in cui il `[Authorize]` e `[AllowAnonymous]` attributi vengono usati nell'app.</span><span class="sxs-lookup"><span data-stu-id="a3c58-224">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="a3c58-225">Le due sezioni seguenti illustrano come gestire gli Stati non consentiti e consentito la configurazione dell'accesso anonimo.</span><span class="sxs-lookup"><span data-stu-id="a3c58-225">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="a3c58-226">Non consentire l'accesso anonimo</span><span class="sxs-lookup"><span data-stu-id="a3c58-226">Disallow anonymous access</span></span>

<span data-ttu-id="a3c58-227">Quando è abilitata l'autenticazione di Windows e accesso anonimo è disabilitato, il `[Authorize]` e `[AllowAnonymous]` attributi non hanno alcun effetto.</span><span class="sxs-lookup"><span data-stu-id="a3c58-227">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="a3c58-228">Se un sito IIS è configurato per non consentire l'accesso anonimo, la richiesta raggiunga mai l'app.</span><span class="sxs-lookup"><span data-stu-id="a3c58-228">If an IIS site is configured to disallow anonymous access, the request never reaches the app.</span></span> <span data-ttu-id="a3c58-229">Per questo motivo, il `[AllowAnonymous]` attributo non è applicabile.</span><span class="sxs-lookup"><span data-stu-id="a3c58-229">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="a3c58-230">Consenti accesso anonimo</span><span class="sxs-lookup"><span data-stu-id="a3c58-230">Allow anonymous access</span></span>

<span data-ttu-id="a3c58-231">Quando sono abilitati sia l'autenticazione di Windows e l'accesso anonimo, usare il `[Authorize]` e `[AllowAnonymous]` attributi.</span><span class="sxs-lookup"><span data-stu-id="a3c58-231">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="a3c58-232">Il `[Authorize]` attributo consente di proteggere gli endpoint dell'app che richiedono l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="a3c58-232">The `[Authorize]` attribute allows you to secure endpoints of the app which require authentication.</span></span> <span data-ttu-id="a3c58-233">Il `[AllowAnonymous]` esegue l'override dell'attributo di `[Authorize]` attributo nelle App che consente l'accesso anonimo.</span><span class="sxs-lookup"><span data-stu-id="a3c58-233">The `[AllowAnonymous]` attribute overrides the `[Authorize]` attribute in apps that allow anonymous access.</span></span> <span data-ttu-id="a3c58-234">Per informazioni dettagliate sull'utilizzo di attributi, vedere <xref:security/authorization/simple>.</span><span class="sxs-lookup"><span data-stu-id="a3c58-234">For attribute usage details, see <xref:security/authorization/simple>.</span></span>

> [!NOTE]
> <span data-ttu-id="a3c58-235">Per impostazione predefinita, gli utenti che non dispongono di autorizzazione per accedere a una pagina vengono visualizzati una risposta HTTP 403 vuota.</span><span class="sxs-lookup"><span data-stu-id="a3c58-235">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="a3c58-236">Il [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) può essere configurato per fornire agli utenti una migliore esperienza di "Accesso negato".</span><span class="sxs-lookup"><span data-stu-id="a3c58-236">The [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) can be configured to provide users with a better "Access Denied" experience.</span></span>

## <a name="impersonation"></a><span data-ttu-id="a3c58-237">Rappresentazione</span><span class="sxs-lookup"><span data-stu-id="a3c58-237">Impersonation</span></span>

<span data-ttu-id="a3c58-238">ASP.NET Core non implementa la rappresentazione.</span><span class="sxs-lookup"><span data-stu-id="a3c58-238">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="a3c58-239">Le app vengono eseguite con l'identità dell'app per tutte le richieste, usando l'identità del pool o un processo dell'app.</span><span class="sxs-lookup"><span data-stu-id="a3c58-239">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="a3c58-240">Se l'app deve eseguire un'azione per conto di un utente, usare [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in un [middleware inline terminal](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="a3c58-240">If the app should perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="a3c58-241">Eseguire una singola azione in questo contesto e quindi chiudere il contesto.</span><span class="sxs-lookup"><span data-stu-id="a3c58-241">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

<span data-ttu-id="a3c58-242">`RunImpersonated` non supporta operazioni asincrone e non deve essere usata per scenari complessi.</span><span class="sxs-lookup"><span data-stu-id="a3c58-242">`RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="a3c58-243">Ad esempio, di wrapping delle richieste intere o catene di middleware, non è supportato o consigliato.</span><span class="sxs-lookup"><span data-stu-id="a3c58-243">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a3c58-244">Mentre il [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) pacchetto consente l'autenticazione in Windows, Linux e macOS, la rappresentazione è supportata solo in Windows.</span><span class="sxs-lookup"><span data-stu-id="a3c58-244">While the [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) package enables authentication on Windows, Linux, and macOS, impersonation is only supported on Windows.</span></span>

::: moniker-end

## <a name="claims-transformations"></a><span data-ttu-id="a3c58-245">Trasformazioni di attestazioni</span><span class="sxs-lookup"><span data-stu-id="a3c58-245">Claims transformations</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a3c58-246">Durante l'hosting con IIS, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> non viene chiamato internamente per inizializzare un utente.</span><span class="sxs-lookup"><span data-stu-id="a3c58-246">When hosting with IIS, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="a3c58-247">Pertanto, un'implementazione di <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> usate per trasformare le attestazioni dopo ogni autenticazione non viene attivata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="a3c58-247">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="a3c58-248">Per altre informazioni e un esempio di codice che attiva le trasformazioni di attestazioni, vedere <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="a3c58-248">For more information and a code example that activates claims transformations, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a3c58-249">Quando si ospitano con la modalità in-process IIS, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> non viene chiamato internamente per inizializzare un utente.</span><span class="sxs-lookup"><span data-stu-id="a3c58-249">When hosting with IIS in-process mode, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="a3c58-250">Pertanto, un'implementazione di <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> usate per trasformare le attestazioni dopo ogni autenticazione non viene attivata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="a3c58-250">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="a3c58-251">Per altre informazioni e un esempio di codice che attiva le trasformazioni di attestazioni per l'hosting in-process, vedere <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="a3c58-251">For more information and a code example that activates claims transformations when hosting in-process, see <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="a3c58-252">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="a3c58-252">Additional resources</span></span>

* [<span data-ttu-id="a3c58-253">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="a3c58-253">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
