---
title: Configurare l'autenticazione di Windows in ASP.NET Core
author: scottaddie
description: Informazioni su come configurare l'autenticazione di Windows in ASP.NET Core, tramite IIS Express, IIS, HTTP. sys e WebListener.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/01/2018
uid: security/authentication/windowsauth
ms.openlocfilehash: 342759a6ff4b5679e0d54c979188ae66d339562d
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121297"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="a65da-103">Configurare l'autenticazione di Windows in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a65da-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="a65da-104">[Steve Smith](https://ardalis.com) e [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="a65da-104">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="a65da-105">L'autenticazione di Windows possono essere configurato per le app ASP.NET Core con IIS, ospitate [HTTP. sys](xref:fundamentals/servers/httpsys), o [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="a65da-105">Windows Authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="windows-authentication"></a><span data-ttu-id="a65da-106">Autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="a65da-106">Windows Authentication</span></span>

<span data-ttu-id="a65da-107">L'autenticazione di Windows si basa sul sistema operativo per autenticare gli utenti delle App ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a65da-107">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="a65da-108">È possibile usare l'autenticazione di Windows quando il server in esecuzione in una rete aziendale usando le identità di dominio Active Directory o altri account di Windows per identificare gli utenti.</span><span class="sxs-lookup"><span data-stu-id="a65da-108">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="a65da-109">L'autenticazione di Windows è più adatta agli ambienti intranet in cui gli utenti, le applicazioni client e server web appartengono allo stesso dominio di Windows.</span><span class="sxs-lookup"><span data-stu-id="a65da-109">Windows Authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="a65da-110">[Altre informazioni sull'autenticazione di Windows e di installarlo per IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="a65da-110">[Learn more about Windows Authentication and installing it for IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="a65da-111">Abilitare l'autenticazione di Windows in un'app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a65da-111">Enable Windows Authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="a65da-112">Il modello di applicazione Web di Visual Studio può essere configurato per supportare l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="a65da-112">The Visual Studio Web Application template can be configured to support Windows Authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="a65da-113">Usare il modello di app di autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="a65da-113">Use the Windows Authentication app template</span></span>

<span data-ttu-id="a65da-114">In Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="a65da-114">In Visual Studio:</span></span>

1. <span data-ttu-id="a65da-115">Creare una nuova applicazione Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a65da-115">Create a new ASP.NET Core Web Application.</span></span>
1. <span data-ttu-id="a65da-116">Selezionare l'applicazione Web dall'elenco dei modelli.</span><span class="sxs-lookup"><span data-stu-id="a65da-116">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="a65da-117">Selezionare il **Modifica autenticazione** e selezionare **l'autenticazione di Windows**.</span><span class="sxs-lookup"><span data-stu-id="a65da-117">Select the **Change Authentication** button and select **Windows Authentication**.</span></span>

<span data-ttu-id="a65da-118">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="a65da-118">Run the app.</span></span> <span data-ttu-id="a65da-119">Il nome utente viene visualizzato nell'angolo superiore destro dell'app.</span><span class="sxs-lookup"><span data-stu-id="a65da-119">The username appears in the top right of the app.</span></span>

![Schermata di Browser l'autenticazione di Windows](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="a65da-121">Per progetti di sviluppo tramite IIS Express, il modello fornisce tutta la configurazione necessaria per usare l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="a65da-121">For development work using IIS Express, the template provides all the configuration necessary to use Windows Authentication.</span></span> <span data-ttu-id="a65da-122">La sezione seguente illustra come configurare manualmente un'app ASP.NET Core per l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="a65da-122">The following section shows how to manually configure an ASP.NET Core app for Windows Authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="a65da-123">Impostazioni di Visual Studio per Windows e l'autenticazione anonima</span><span class="sxs-lookup"><span data-stu-id="a65da-123">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="a65da-124">Il progetto di Visual Studio **delle proprietà** della pagina **Debug** scheda fornisce caselle di controllo per l'autenticazione di Windows e l'autenticazione anonima.</span><span class="sxs-lookup"><span data-stu-id="a65da-124">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows Authentication and anonymous authentication.</span></span>

![Schermata del Browser l'autenticazione di Windows con le opzioni di autenticazione evidenziate](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="a65da-126">In alternativa, è possibile configurare queste due proprietà nel *launchsettings. JSON* file:</span><span class="sxs-lookup"><span data-stu-id="a65da-126">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="a65da-127">Abilitare l'autenticazione di Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="a65da-127">Enable Windows Authentication with IIS</span></span>

<span data-ttu-id="a65da-128">IIS Usa il [modulo di ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) per ospitare App ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a65da-128">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="a65da-129">L'autenticazione di Windows è configurata in IIS, non l'app.</span><span class="sxs-lookup"><span data-stu-id="a65da-129">Windows Authentication is configured in IIS, not the app.</span></span> <span data-ttu-id="a65da-130">Le sezioni seguenti illustrano come usare Gestione IIS per configurare un'app ASP.NET Core per usare l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="a65da-130">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows Authentication.</span></span>

### <a name="iis-configuration"></a><span data-ttu-id="a65da-131">Configurazione di IIS</span><span class="sxs-lookup"><span data-stu-id="a65da-131">IIS configuration</span></span>

<span data-ttu-id="a65da-132">Abilitare il servizio ruolo IIS per l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="a65da-132">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="a65da-133">Per altre informazioni, vedere [abilitare l'autenticazione di Windows nei servizi di ruolo IIS (vedere il passaggio 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="a65da-133">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="a65da-134">Per impostazione predefinita, il Middleware di integrazione IIS è configurato per autenticare automaticamente le richieste.</span><span class="sxs-lookup"><span data-stu-id="a65da-134">IIS Integration Middleware is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="a65da-135">Per altre informazioni, vedere [Host ASP.NET Core in Windows con IIS: le opzioni di IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="a65da-135">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="a65da-136">Per impostazione predefinita, il modulo ASP.NET Core è configurato per inoltrare il token di autenticazione di Windows per l'app.</span><span class="sxs-lookup"><span data-stu-id="a65da-136">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="a65da-137">Per altre informazioni, vedere [riferimento configurazione modulo ASP.NET Core: attributi dell'elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="a65da-137">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="a65da-138">Creare un nuovo sito IIS</span><span class="sxs-lookup"><span data-stu-id="a65da-138">Create a new IIS site</span></span>

<span data-ttu-id="a65da-139">Specificare un nome e una cartella e consentirgli di creare un nuovo pool di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="a65da-139">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="a65da-140">Personalizzare l'autenticazione</span><span class="sxs-lookup"><span data-stu-id="a65da-140">Customize authentication</span></span>

<span data-ttu-id="a65da-141">Aprire le funzionalità di autenticazione per il sito.</span><span class="sxs-lookup"><span data-stu-id="a65da-141">Open the Authentication features for the site.</span></span>

![Menu di scelta l'autenticazione IIS](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="a65da-143">Disabilitare l'autenticazione anonima e abilitare l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="a65da-143">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![Impostazioni di autenticazione IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="a65da-145">Pubblicare il progetto alla cartella del sito IIS</span><span class="sxs-lookup"><span data-stu-id="a65da-145">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="a65da-146">Usa Visual Studio o .NET Core CLI, pubblicare l'app per la cartella di destinazione.</span><span class="sxs-lookup"><span data-stu-id="a65da-146">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Finestra di dialogo di pubblicazione di Visual Studio](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="a65da-148">Altre informazioni sulle [pubblicazione in IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="a65da-148">Learn more about [publishing to IIS](xref:host-and-deploy/iis/index).</span></span>

<span data-ttu-id="a65da-149">Avviare l'app per verificare l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="a65da-149">Launch the app to verify Windows Authentication is working.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="enable-windows-authentication-with-httpsys"></a><span data-ttu-id="a65da-150">Abilitare l'autenticazione di Windows con HTTP. sys</span><span class="sxs-lookup"><span data-stu-id="a65da-150">Enable Windows Authentication with HTTP.sys</span></span>

<span data-ttu-id="a65da-151">Anche se Kestrel non supporta l'autenticazione di Windows, è possibile usare [HTTP. sys](xref:fundamentals/servers/httpsys) per supportare gli scenari self-hosted in Windows.</span><span class="sxs-lookup"><span data-stu-id="a65da-151">Although Kestrel doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="a65da-152">L'esempio seguente configura l'host web dell'app per usare HTTP. sys con l'autenticazione di Windows:</span><span class="sxs-lookup"><span data-stu-id="a65da-152">The following example configures the app's web host to use HTTP.sys with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

> [!NOTE]
> <span data-ttu-id="a65da-153">Per la delega all'autenticazione in modalità kernel, HTTP.sys usa il protocollo di autenticazione Kerberos.</span><span class="sxs-lookup"><span data-stu-id="a65da-153">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="a65da-154">L'autenticazione in modalità utente non è supportata con Kerberos e HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="a65da-154">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="a65da-155">È necessario usare l'account del computer per decrittografare il token/ticket Kerberos ottenuto da Active Directory e inoltrato dal client al server per autenticare l'utente.</span><span class="sxs-lookup"><span data-stu-id="a65da-155">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="a65da-156">Registrare il nome dell'entità servizio per l'host, non l'utente dell'app.</span><span class="sxs-lookup"><span data-stu-id="a65da-156">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="a65da-157">Http. sys non è supportata in Nano Server versione 1709 o successiva.</span><span class="sxs-lookup"><span data-stu-id="a65da-157">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="a65da-158">Per usare l'autenticazione di Windows e HTTP. sys con Nano Server, usare una [contenitore di Server Core (microsoft/windowsservercore)](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="a65da-158">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="a65da-159">Per altre informazioni su Server Core, vedere [qual è l'opzione di installazione Server Core in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="a65da-159">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

## <a name="enable-windows-authentication-with-weblistener"></a><span data-ttu-id="a65da-160">Abilitare l'autenticazione di Windows con WebListener</span><span class="sxs-lookup"><span data-stu-id="a65da-160">Enable Windows Authentication with WebListener</span></span>

<span data-ttu-id="a65da-161">Anche se Kestrel non supporta l'autenticazione di Windows, è possibile usare [WebListener](xref:fundamentals/servers/weblistener) per supportare gli scenari self-hosted in Windows.</span><span class="sxs-lookup"><span data-stu-id="a65da-161">Although Kestrel doesn't support Windows Authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="a65da-162">L'esempio seguente configura l'host web dell'app per usare WebListener con l'autenticazione di Windows:</span><span class="sxs-lookup"><span data-stu-id="a65da-162">The following example configures the app's web host to use WebListener with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

> [!NOTE]
> <span data-ttu-id="a65da-163">Per la delega all'autenticazione in modalità kernel, WebListener usa il protocollo di autenticazione Kerberos.</span><span class="sxs-lookup"><span data-stu-id="a65da-163">WebListener delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="a65da-164">L'autenticazione in modalità utente non è supportata con Kerberos e WebListener.</span><span class="sxs-lookup"><span data-stu-id="a65da-164">User mode authentication isn't supported with Kerberos and WebListener.</span></span> <span data-ttu-id="a65da-165">È necessario usare l'account del computer per decrittografare il token/ticket Kerberos ottenuto da Active Directory e inoltrato dal client al server per autenticare l'utente.</span><span class="sxs-lookup"><span data-stu-id="a65da-165">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="a65da-166">Registrare il nome dell'entità servizio per l'host, non l'utente dell'app.</span><span class="sxs-lookup"><span data-stu-id="a65da-166">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

::: moniker-end

## <a name="work-with-windows-authentication"></a><span data-ttu-id="a65da-167">Utilizzo con l'autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="a65da-167">Work with Windows Authentication</span></span>

<span data-ttu-id="a65da-168">Lo stato di configurazione dell'accesso anonimo determina il modo in cui il `[Authorize]` e `[AllowAnonymous]` attributi vengono usati nell'app.</span><span class="sxs-lookup"><span data-stu-id="a65da-168">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="a65da-169">Le due sezioni seguenti illustrano come gestire gli Stati non consentiti e consentito la configurazione dell'accesso anonimo.</span><span class="sxs-lookup"><span data-stu-id="a65da-169">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="a65da-170">Non consentire l'accesso anonimo</span><span class="sxs-lookup"><span data-stu-id="a65da-170">Disallow anonymous access</span></span>

<span data-ttu-id="a65da-171">Quando è abilitata l'autenticazione di Windows e accesso anonimo è disabilitato, il `[Authorize]` e `[AllowAnonymous]` attributi non hanno alcun effetto.</span><span class="sxs-lookup"><span data-stu-id="a65da-171">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="a65da-172">Se il sito IIS (o server HTTP. sys o WebListener) è configurato per non consentire l'accesso anonimo, la richiesta raggiunga mai l'app.</span><span class="sxs-lookup"><span data-stu-id="a65da-172">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="a65da-173">Per questo motivo, il `[AllowAnonymous]` attributo non è applicabile.</span><span class="sxs-lookup"><span data-stu-id="a65da-173">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="a65da-174">Consenti accesso anonimo</span><span class="sxs-lookup"><span data-stu-id="a65da-174">Allow anonymous access</span></span>

<span data-ttu-id="a65da-175">Quando sono abilitati sia l'autenticazione di Windows e l'accesso anonimo, usare il `[Authorize]` e `[AllowAnonymous]` attributi.</span><span class="sxs-lookup"><span data-stu-id="a65da-175">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="a65da-176">Il `[Authorize]` attributo consente di proteggere parti dell'app che realmente richiedono l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="a65da-176">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows Authentication.</span></span> <span data-ttu-id="a65da-177">Il `[AllowAnonymous]` esegue l'override dell'attributo `[Authorize]` utilizzo all'interno delle App che consentono l'accesso anonimo degli attributi.</span><span class="sxs-lookup"><span data-stu-id="a65da-177">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="a65da-178">Visualizzare [autorizzazione semplice](xref:security/authorization/simple) per informazioni dettagliate sull'utilizzo di attributi.</span><span class="sxs-lookup"><span data-stu-id="a65da-178">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="a65da-179">In ASP.NET Core 2.x, il `[Authorize]` attributo richiede una configurazione aggiuntiva nel *Startup.cs* per stimolare le richieste anonime per l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="a65da-179">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows Authentication.</span></span> <span data-ttu-id="a65da-180">La configurazione consigliata varia leggermente in base sul server web in uso.</span><span class="sxs-lookup"><span data-stu-id="a65da-180">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="a65da-181">Per impostazione predefinita, gli utenti che non dispongono di autorizzazione per accedere a una pagina vengono visualizzati una risposta HTTP 403 vuota.</span><span class="sxs-lookup"><span data-stu-id="a65da-181">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="a65da-182">Il [StatusCodePages middleware](xref:fundamentals/error-handling#configure-status-code-pages) può essere configurato per fornire agli utenti una migliore esperienza di "Accesso negato".</span><span class="sxs-lookup"><span data-stu-id="a65da-182">The [StatusCodePages middleware](xref:fundamentals/error-handling#configure-status-code-pages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="a65da-183">IIS</span><span class="sxs-lookup"><span data-stu-id="a65da-183">IIS</span></span>

<span data-ttu-id="a65da-184">Se si usa IIS, aggiungere il codice seguente il `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="a65da-184">If using IIS, add the following to the `ConfigureServices` method:</span></span>

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="a65da-185">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="a65da-185">HTTP.sys</span></span>

<span data-ttu-id="a65da-186">Se si Usa HTTP. sys, aggiungere il codice seguente il `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="a65da-186">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="a65da-187">Rappresentazione</span><span class="sxs-lookup"><span data-stu-id="a65da-187">Impersonation</span></span>

<span data-ttu-id="a65da-188">ASP.NET Core non implementa la rappresentazione.</span><span class="sxs-lookup"><span data-stu-id="a65da-188">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="a65da-189">Le app vengono eseguite con l'identità dell'applicazione per tutte le richieste, usando l'identità del pool o un processo dell'app.</span><span class="sxs-lookup"><span data-stu-id="a65da-189">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="a65da-190">Se è necessario eseguire in modo esplicito un'azione per conto di un utente, usare `WindowsIdentity.RunImpersonated`.</span><span class="sxs-lookup"><span data-stu-id="a65da-190">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="a65da-191">Eseguire una singola azione in questo contesto e quindi chiudere il contesto.</span><span class="sxs-lookup"><span data-stu-id="a65da-191">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="a65da-192">Si noti che `RunImpersonated` non supporta operazioni asincrone e non deve essere usata per scenari complessi.</span><span class="sxs-lookup"><span data-stu-id="a65da-192">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="a65da-193">Ad esempio, di wrapping delle richieste intere o catene di middleware, non è supportato o consigliato.</span><span class="sxs-lookup"><span data-stu-id="a65da-193">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>
