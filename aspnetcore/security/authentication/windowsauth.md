---
title: Configurare l'autenticazione di Windows in ASP.NET Core
author: ardalis
description: In questo articolo viene descritto come configurare l'autenticazione di Windows in ASP.NET Core, utilizzando IIS Express, IIS, HTTP.sys e WebListener.
manager: wpickett
ms.author: riande
ms.date: 10/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/windowsauth
ms.openlocfilehash: f6efd838d7b6c837c75f36591a49eab812f9d54c
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/02/2018
---
# <a name="configure-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="bc3a4-103">Configurare l'autenticazione di Windows in un'applicazione ASP.NET di base</span><span class="sxs-lookup"><span data-stu-id="bc3a4-103">Configure Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="bc3a4-104">[Steve Smith](https://ardalis.com) e [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="bc3a4-104">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="bc3a4-105">L'autenticazione di Windows può essere configurata per le applicazioni ASP.NET Core ospitate in IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), o [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="bc3a4-105">Windows authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="what-is-windows-authentication"></a><span data-ttu-id="bc3a4-106">Che cos'è l'autenticazione di Windows?</span><span class="sxs-lookup"><span data-stu-id="bc3a4-106">What is Windows authentication?</span></span>

<span data-ttu-id="bc3a4-107">L'autenticazione di Windows si basa sul sistema operativo per autenticare gli utenti delle applicazioni ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-107">Windows authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="bc3a4-108">Quando il server viene eseguito in una rete aziendale usando le identità di dominio Active Directory o altri account di Windows per identificare gli utenti, è possibile utilizzare l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-108">You can use Windows authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="bc3a4-109">L'autenticazione di Windows è più adatta agli ambienti intranet in cui gli utenti, le applicazioni client e server web appartengono allo stesso dominio di Windows.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-109">Windows authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="bc3a4-110">[Ulteriori informazioni sull'autenticazione di Windows e l'installazione di IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="bc3a4-110">[Learn more about Windows authentication and installing it for IIS](https://docs.microsoft.com/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="bc3a4-111">Abilitare l'autenticazione di Windows in un'applicazione ASP.NET di base</span><span class="sxs-lookup"><span data-stu-id="bc3a4-111">Enable Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="bc3a4-112">Il modello di applicazione Web di Visual Studio può essere configurato per supportare l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-112">The Visual Studio Web Application template can be configured to support Windows authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="bc3a4-113">Utilizzare il modello di app di autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="bc3a4-113">Use the Windows authentication app template</span></span>

<span data-ttu-id="bc3a4-114">In Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="bc3a4-114">In Visual Studio:</span></span>
1. <span data-ttu-id="bc3a4-115">Creare una nuova applicazione Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-115">Create a new ASP.NET Core Web Application.</span></span> 
1. <span data-ttu-id="bc3a4-116">Selezionare l'applicazione Web dall'elenco dei modelli.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-116">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="bc3a4-117">Selezionare il **Modifica autenticazione** e selezionare **l'autenticazione di Windows**.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-117">Select the **Change Authentication** button and select **Windows Authentication**.</span></span> 

<span data-ttu-id="bc3a4-118">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-118">Run the app.</span></span> <span data-ttu-id="bc3a4-119">Il nome utente viene visualizzato nella parte superiore destra dell'app.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-119">The username appears in the top right of the app.</span></span>

![Schermata di Browser l'autenticazione di Windows](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="bc3a4-121">Per progetti di sviluppo utilizzando IIS Express, il modello fornisce tutte la configurazione necessaria per utilizzare l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-121">For development work using IIS Express, the template provides all the configuration necessary to use Windows authentication.</span></span> <span data-ttu-id="bc3a4-122">Nella sezione seguente viene illustrato come configurare manualmente un'applicazione ASP.NET Core per l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-122">The following section shows how to manually configure an ASP.NET Core app for Windows authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="bc3a4-123">Impostazioni di Visual Studio per Windows e l'autenticazione anonima</span><span class="sxs-lookup"><span data-stu-id="bc3a4-123">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="bc3a4-124">Il progetto di Visual Studio **proprietà** della pagina **Debug** scheda fornisce caselle di controllo per l'autenticazione di Windows e l'autenticazione anonima.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-124">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows authentication and anonymous authentication.</span></span>

![Schermata di Browser l'autenticazione di Windows](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="bc3a4-126">In alternativa, queste due proprietà possono essere configurate nel *launchSettings.json* file:</span><span class="sxs-lookup"><span data-stu-id="bc3a4-126">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="bc3a4-127">Abilitare l'autenticazione di Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="bc3a4-127">Enable Windows authentication with IIS</span></span>

<span data-ttu-id="bc3a4-128">IIS Usa il [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module) alle App ASP.NET Core host.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-128">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="bc3a4-129">Il modulo flussi l'autenticazione di Windows in IIS per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-129">The module flows Windows authentication to IIS by default.</span></span> <span data-ttu-id="bc3a4-130">L'autenticazione di Windows è configurata in IIS, non l'app.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-130">Windows authentication is configured in IIS, not the app.</span></span> <span data-ttu-id="bc3a4-131">Nelle sezioni seguenti viene illustrato come utilizzare Gestione IIS per configurare un'app di ASP.NET Core per utilizzare l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-131">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows authentication.</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="bc3a4-132">Creare un nuovo sito IIS</span><span class="sxs-lookup"><span data-stu-id="bc3a4-132">Create a new IIS site</span></span>

<span data-ttu-id="bc3a4-133">Specificare un nome e una cartella e consentono di creare un nuovo pool di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-133">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="bc3a4-134">Personalizzare l'autenticazione</span><span class="sxs-lookup"><span data-stu-id="bc3a4-134">Customize authentication</span></span>

<span data-ttu-id="bc3a4-135">Aprire il menu di autenticazione per il sito.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-135">Open the Authentication menu for the site.</span></span>

![Menu di autenticazione IIS](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="bc3a4-137">Disabilitare l'autenticazione anonima e abilitare l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-137">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![Impostazioni di autenticazione IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="bc3a4-139">Pubblicare il progetto alla cartella del sito IIS</span><span class="sxs-lookup"><span data-stu-id="bc3a4-139">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="bc3a4-140">Con Visual Studio o .NET Core CLI, pubblicare l'app nella cartella di destinazione.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-140">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Finestra di dialogo di pubblicazione di Visual Studio](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="bc3a4-142">Altre informazioni, vedere [la pubblicazione in IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="bc3a4-142">Learn more about [publishing to IIS](xref:host-and-deploy/iis/index).</span></span>

<span data-ttu-id="bc3a4-143">Avviare l'app per verificare l'autenticazione di Windows sia funzionante.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-143">Launch the app to verify Windows authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a><span data-ttu-id="bc3a4-144">Abilitare l'autenticazione di Windows con HTTP.sys o WebListener</span><span class="sxs-lookup"><span data-stu-id="bc3a4-144">Enable Windows authentication with HTTP.sys or WebListener</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bc3a4-145">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bc3a4-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="bc3a4-146">Sebbene Kestrel non supporta l'autenticazione di Windows, è possibile utilizzare [HTTP.sys](xref:fundamentals/servers/httpsys) per supportare gli scenari indipendenti che in Windows.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-146">Although Kestrel doesn't support Windows authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="bc3a4-147">Nell'esempio seguente consente di configurare dell'host dell'applicazione web per l'utilizzo di HTTP.sys con l'autenticazione di Windows:</span><span class="sxs-lookup"><span data-stu-id="bc3a4-147">The following example configures the app's web host to use HTTP.sys with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bc3a4-148">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bc3a4-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="bc3a4-149">Sebbene Kestrel non supporta l'autenticazione di Windows, è possibile utilizzare [WebListener](xref:fundamentals/servers/weblistener) per supportare gli scenari indipendenti che in Windows.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-149">Although Kestrel doesn't support Windows authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="bc3a4-150">Nell'esempio seguente consente di configurare dell'host dell'applicazione web per utilizzare WebListener con l'autenticazione di Windows:</span><span class="sxs-lookup"><span data-stu-id="bc3a4-150">The following example configures the app's web host to use WebListener with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a><span data-ttu-id="bc3a4-151">Usare l'autenticazione di Windows</span><span class="sxs-lookup"><span data-stu-id="bc3a4-151">Work with Windows authentication</span></span>

<span data-ttu-id="bc3a4-152">Lo stato di configurazione dell'accesso anonimo determina il modo in cui il `[Authorize]` e `[AllowAnonymous]` attributi vengono usati nell'app.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-152">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="bc3a4-153">Nelle due sezioni seguenti illustrano come gestire gli stati di configurazione non consentite e sono consentite dell'accesso anonimo.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-153">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="bc3a4-154">Negare l'accesso anonimo</span><span class="sxs-lookup"><span data-stu-id="bc3a4-154">Disallow anonymous access</span></span>

<span data-ttu-id="bc3a4-155">Quando è abilitata l'autenticazione di Windows e accesso anonimo è disabilitato, il `[Authorize]` e `[AllowAnonymous]` gli attributi non hanno alcun effetto.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-155">When Windows authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="bc3a4-156">Se il sito IIS (o server HTTP. sys o WebListener) è configurato per negare l'accesso anonimo, la richiesta raggiunga mai l'app.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-156">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="bc3a4-157">Per questo motivo, il `[AllowAnonymous]` attributo non è applicabile.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-157">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="bc3a4-158">Consenti accesso anonimo</span><span class="sxs-lookup"><span data-stu-id="bc3a4-158">Allow anonymous access</span></span>

<span data-ttu-id="bc3a4-159">Quando sono abilitati sia l'autenticazione di Windows e l'accesso anonimo, usare il `[Authorize]` e `[AllowAnonymous]` gli attributi.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-159">When both Windows authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="bc3a4-160">Il `[Authorize]` attributo consente di proteggere parti dell'app che richiedono effettivamente l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-160">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows authentication.</span></span> <span data-ttu-id="bc3a4-161">Il `[AllowAnonymous]` esegue l'override dell'attributo `[Authorize]` attributo utilizzo all'interno di applicazioni che consentono l'accesso anonimo.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-161">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="bc3a4-162">Vedere [autorizzazione semplice](xref:security/authorization/simple) per informazioni dettagliate sull'utilizzo di attributi.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-162">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="bc3a4-163">In ASP.NET Core 2. x, il `[Authorize]` attributo richiede una configurazione aggiuntiva in *Startup.cs* di incentivare le richieste anonime per l'autenticazione di Windows.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-163">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows authentication.</span></span> <span data-ttu-id="bc3a4-164">La configurazione consigliata varia leggermente in base al server web in uso.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-164">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="bc3a4-165">Per impostazione predefinita, gli utenti che non dispongono di autorizzazioni per accedere a una pagina vengono visualizzati una risposta HTTP 403 vuota.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-165">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="bc3a4-166">Il [StatusCodePages middleware](xref:fundamentals/error-handling#configuring-status-code-pages) può essere configurato per fornire agli utenti un'esperienza migliore "Accesso negato".</span><span class="sxs-lookup"><span data-stu-id="bc3a4-166">The [StatusCodePages middleware](xref:fundamentals/error-handling#configuring-status-code-pages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="bc3a4-167">IIS</span><span class="sxs-lookup"><span data-stu-id="bc3a4-167">IIS</span></span>

<span data-ttu-id="bc3a4-168">Se si utilizza IIS, aggiungere il comando seguente per il `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="bc3a4-168">If using IIS, add the following to the `ConfigureServices` method:</span></span> 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="bc3a4-169">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="bc3a4-169">HTTP.sys</span></span>

<span data-ttu-id="bc3a4-170">Se si utilizza HTTP.sys, aggiungere il comando seguente per il `ConfigureServices` metodo:</span><span class="sxs-lookup"><span data-stu-id="bc3a4-170">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="bc3a4-171">Rappresentazione</span><span class="sxs-lookup"><span data-stu-id="bc3a4-171">Impersonation</span></span>

<span data-ttu-id="bc3a4-172">ASP.NET Core non implementa la rappresentazione.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-172">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="bc3a4-173">Le app vengono eseguite con l'identità di applicazione per tutte le richieste, utilizzando l'identità di processo o i pool di app.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-173">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="bc3a4-174">Se si desidera eseguire in modo esplicito un'azione per conto dell'utente, utilizzare `WindowsIdentity.RunImpersonated`.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-174">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="bc3a4-175">Eseguire un'unica azione in questo contesto e quindi chiudere il contesto.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-175">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="bc3a4-176">Si noti che `RunImpersonated` non supporta le operazioni asincrone e non devono essere usate per scenari complessi.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-176">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="bc3a4-177">Ad esempio, il wrapping di richieste intere o middleware catene, non è supportato o consigliato.</span><span class="sxs-lookup"><span data-stu-id="bc3a4-177">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

---
