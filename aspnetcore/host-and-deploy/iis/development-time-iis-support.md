---
title: Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core
author: shirhatti
description: Informazioni sul supporto del debug di app ASP.NET Core durante l'esecuzione dietro IIS in Windows Server.
ms.author: riande
ms.custom: mvc
ms.date: 11/30/2018
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 51375e6a6bb25a469d467ca97a151abd305c1ece
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52862382"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="f8455-103">Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f8455-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="f8455-104">Di [Sourabh Shirhatti](https://twitter.com/sshirhatti) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f8455-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f8455-105">Questo articolo descrive il supporto del debug di app ASP.NET Core in [Visual Studio](https://www.visualstudio.com/vs/) durante l'esecuzione dietro IIS in Windows Server.</span><span class="sxs-lookup"><span data-stu-id="f8455-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="f8455-106">Questo argomento illustra nel dettaglio l'abilitazione della funzionalità e la configurazione di un progetto.</span><span class="sxs-lookup"><span data-stu-id="f8455-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f8455-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="f8455-107">Prerequisites</span></span>

* [<span data-ttu-id="f8455-108">Visual Studio per Windows</span><span class="sxs-lookup"><span data-stu-id="f8455-108">Visual Studio for Windows</span></span>](https://www.microsoft.com/net/download/windows)
* <span data-ttu-id="f8455-109">**ASP.NET e carico di lavoro di sviluppo Web**</span><span class="sxs-lookup"><span data-stu-id="f8455-109">**ASP.NET and web development** workload</span></span>
* <span data-ttu-id="f8455-110">Carico di lavoro di **sviluppo multipiattaforma .NET Core**</span><span class="sxs-lookup"><span data-stu-id="f8455-110">**.NET Core cross-platform development** workload</span></span>
* <span data-ttu-id="f8455-111">Certificato di protezione X.509</span><span class="sxs-lookup"><span data-stu-id="f8455-111">X.509 security certificate</span></span>

## <a name="enable-iis"></a><span data-ttu-id="f8455-112">Abilitare IIS</span><span class="sxs-lookup"><span data-stu-id="f8455-112">Enable IIS</span></span>

1. <span data-ttu-id="f8455-113">Passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo).</span><span class="sxs-lookup"><span data-stu-id="f8455-113">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="f8455-114">Selezionare la casella di controllo **Internet Information Services**.</span><span class="sxs-lookup"><span data-stu-id="f8455-114">Select the **Internet Information Services** check box.</span></span>

![Funzionalità di Windows con la casella di controllo Internet Information Services selezionata come quadrato nero (non con un segno di spunta) ad indicare che alcune funzionalità di IIS sono abilitate](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="f8455-116">L'installazione di IIS potrebbe richiedere un riavvio del sistema.</span><span class="sxs-lookup"><span data-stu-id="f8455-116">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="f8455-117">Configura IIS</span><span class="sxs-lookup"><span data-stu-id="f8455-117">Configure IIS</span></span>

<span data-ttu-id="f8455-118">IIS deve disporre di un sito Web configurato con gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f8455-118">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="f8455-119">Nome host che corrisponde al nome host dell'URL del profilo di avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="f8455-119">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="f8455-120">Binding per la porta 443 con un certificato assegnato.</span><span class="sxs-lookup"><span data-stu-id="f8455-120">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="f8455-121">Ad esempio, il **nome host** per un sito Web aggiunto è impostato su "localhost" (anche il profilo di avvio userà "localhost" più avanti in questo argomento).</span><span class="sxs-lookup"><span data-stu-id="f8455-121">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="f8455-122">La porta è impostata su "443" (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="f8455-122">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="f8455-123">Al sito Web viene assegnato il **certificato di sviluppo di IIS Express**, ma può essere usato qualsiasi certificato valido:</span><span class="sxs-lookup"><span data-stu-id="f8455-123">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![Aggiungere la finestra del sito Web in IIS, in cui è visualizzato il binding impostato per localhost sulla porta 443 con un certificato assegnato.](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="f8455-125">Se l'installazione di IIS dispone già di un **sito Web predefinito** con un nome host che corrisponde al nome host dell'URL del profilo di avvio dell'app:</span><span class="sxs-lookup"><span data-stu-id="f8455-125">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="f8455-126">Aggiungere un binding per la porta 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="f8455-126">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="f8455-127">Assegnare un certificato valido al sito Web.</span><span class="sxs-lookup"><span data-stu-id="f8455-127">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="f8455-128">Abilitare il supporto di IIS in fase di sviluppo in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8455-128">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="f8455-129">Avviare il programma di installazione di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f8455-129">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="f8455-130">Selezionare il componente **Supporto IIS in fase di sviluppo**.</span><span class="sxs-lookup"><span data-stu-id="f8455-130">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="f8455-131">Il componente è elencato come facoltativo nel pannello **Riepilogo** del carico di lavoro **Sviluppo ASP.NET e Web**.</span><span class="sxs-lookup"><span data-stu-id="f8455-131">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="f8455-132">Il componente installa il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), un modulo IIS nativo necessario per l'esecuzione delle applicazioni ASP.NET Core con IIS.</span><span class="sxs-lookup"><span data-stu-id="f8455-132">The component installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps with IIS.</span></span>

![Modifica delle funzionalità di Visual Studio: la scheda Carichi di lavoro è selezionata.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="f8455-136">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="f8455-136">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="f8455-137">Reindirizzamento HTTPS</span><span class="sxs-lookup"><span data-stu-id="f8455-137">HTTPS redirection</span></span>

<span data-ttu-id="f8455-138">Per un nuovo progetto, selezionare la casella di controllo **Configura per HTTPS** nella finestra **Nuova applicazione Web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="f8455-138">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![Finestra Nuova applicazione Web ASP.NET Core con la casella di controllo Configura per HTTPS selezionata.](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="f8455-140">In un progetto esistente, usare il middleware di reindirizzamento HTTPS in `Startup.Configure` chiamando il metodo di estensione [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection):</span><span class="sxs-lookup"><span data-stu-id="f8455-140">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();

    app.UseMvc();
}
```

### <a name="iis-launch-profile"></a><span data-ttu-id="f8455-141">Profilo di avvio IIS</span><span class="sxs-lookup"><span data-stu-id="f8455-141">IIS launch profile</span></span>

<span data-ttu-id="f8455-142">Creare un nuovo profilo di avvio per aggiungere il supporto IIS in fase di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="f8455-142">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="f8455-143">Per **Profilo** selezionare il pulsante **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="f8455-143">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="f8455-144">Denominare il profilo "IIS" nella finestra popup.</span><span class="sxs-lookup"><span data-stu-id="f8455-144">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="f8455-145">Selezionare **OK** per creare il profilo.</span><span class="sxs-lookup"><span data-stu-id="f8455-145">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="f8455-146">Per l'impostazione **Avvio** selezionare **IIS** dall'elenco.</span><span class="sxs-lookup"><span data-stu-id="f8455-146">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="f8455-147">Selezionare la casella di controllo **Avvia browser** e specificare l'URL dell'endpoint.</span><span class="sxs-lookup"><span data-stu-id="f8455-147">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="f8455-148">Usare il protocollo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f8455-148">Use the HTTPS protocol.</span></span> <span data-ttu-id="f8455-149">In questo esempio viene usato `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="f8455-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="f8455-150">Nella sezione **Variabili di ambiente** selezionare il pulsante **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f8455-150">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="f8455-151">Specificare una variabile di ambiente con una chiave `ASPNETCORE_ENVIRONMENT` e il valore `Development`.</span><span class="sxs-lookup"><span data-stu-id="f8455-151">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="f8455-152">Nell'area **Impostazioni server Web** impostare **URL App**.</span><span class="sxs-lookup"><span data-stu-id="f8455-152">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="f8455-153">In questo esempio viene usato `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="f8455-153">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="f8455-154">Salvare il profilo.</span><span class="sxs-lookup"><span data-stu-id="f8455-154">Save the profile.</span></span>

![Finestra delle proprietà del progetto con la scheda Debug selezionata.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="f8455-159">In alternativa, aggiungere manualmente un profilo di avvio al file [launchSettings.json](http://json.schemastore.org/launchsettings) nell'app:</span><span class="sxs-lookup"><span data-stu-id="f8455-159">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

## <a name="run-the-project"></a><span data-ttu-id="f8455-160">Eseguire il progetto</span><span class="sxs-lookup"><span data-stu-id="f8455-160">Run the project</span></span>

<span data-ttu-id="f8455-161">In Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="f8455-161">In Visual Studio:</span></span>

* <span data-ttu-id="f8455-162">Verificare che l'elenco di riepilogo a discesa della configurazione della build sia impostato su **Debug**.</span><span class="sxs-lookup"><span data-stu-id="f8455-162">Confirm that the build configuration drop-down list is set to **Debug**.</span></span>
* <span data-ttu-id="f8455-163">Impostare il pulsante Esegui sul profilo **IIS** e selezionare il pulsante per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="f8455-163">Set the Run button to the **IIS** profile and select the button to start the app.</span></span>

![Il pulsante Esegui sulla barra degli strumenti di Visual Studio viene impostato sul profilo IIS con l'elenco di riepilogo a discesa della configurazione della build impostato su Versione.](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="f8455-165">Visual Studio potrebbe richiedere un riavvio, se non si è in modalità amministratore.</span><span class="sxs-lookup"><span data-stu-id="f8455-165">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="f8455-166">In tal caso riavviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f8455-166">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="f8455-167">Se viene usato un certificato di sviluppo non attendibile, il browser potrebbe richiedere di creare un'eccezione per il certificato non attendibile.</span><span class="sxs-lookup"><span data-stu-id="f8455-167">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

> [!NOTE]
> <span data-ttu-id="f8455-168">Il debug della configurazione della build Versione con [Just My Code](/visualstudio/debugger/just-my-code) e le ottimizzazioni del compilatore genera un'esperienza con funzionalità ridotta.</span><span class="sxs-lookup"><span data-stu-id="f8455-168">Debugging a Release build configuration with [Just My Code](/visualstudio/debugger/just-my-code) and compiler optimizations results in a degraded experience.</span></span> <span data-ttu-id="f8455-169">Ad esempio, i punti di interruzione non vengono raggiunti.</span><span class="sxs-lookup"><span data-stu-id="f8455-169">For example, break points aren't hit.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f8455-170">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f8455-170">Additional resources</span></span>

* [<span data-ttu-id="f8455-171">Host ASP.NET Core in Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="f8455-171">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="f8455-172">Introduzione al modulo di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f8455-172">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="f8455-173">Guida di riferimento per la configurazione del modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f8455-173">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="f8455-174">Imporre HTTPS</span><span class="sxs-lookup"><span data-stu-id="f8455-174">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
