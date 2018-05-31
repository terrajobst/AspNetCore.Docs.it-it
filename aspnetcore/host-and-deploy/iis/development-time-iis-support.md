---
title: Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core
author: shirhatti
description: Informazioni sul supporto del debug di app ASP.NET Core durante l'esecuzione dietro IIS in Windows Server.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 0bf4585d44e61c5e7e5b89ce9d8dfdfa10d5460e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/17/2018
ms.locfileid: "34233079"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="1ebd7-103">Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ebd7-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="1ebd7-104">Di [Sourabh Shirhatti](https://twitter.com/sshirhatti) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1ebd7-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1ebd7-105">Questo articolo descrive il supporto del debug di app ASP.NET Core in [Visual Studio](https://www.visualstudio.com/vs/) durante l'esecuzione dietro IIS in Windows Server.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="1ebd7-106">Questo argomento illustra nel dettaglio l'abilitazione della funzionalità e la configurazione di un progetto.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1ebd7-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="1ebd7-107">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="1ebd7-108">Abilitare IIS</span><span class="sxs-lookup"><span data-stu-id="1ebd7-108">Enable IIS</span></span>

1. <span data-ttu-id="1ebd7-109">Passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo).</span><span class="sxs-lookup"><span data-stu-id="1ebd7-109">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="1ebd7-110">Selezionare la casella di controllo **Internet Information Services**.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-110">Select the **Internet Information Services** check box.</span></span>

![Funzionalità di Windows con la casella di controllo Internet Information Services selezionata come quadrato nero (non con un segno di spunta) ad indicare che alcune funzionalità di IIS sono abilitate](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="1ebd7-112">L'installazione di IIS potrebbe richiedere un riavvio del sistema.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-112">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="1ebd7-113">Configura IIS</span><span class="sxs-lookup"><span data-stu-id="1ebd7-113">Configure IIS</span></span>

<span data-ttu-id="1ebd7-114">IIS deve disporre di un sito Web configurato con gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="1ebd7-114">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="1ebd7-115">Nome host che corrisponde al nome host dell'URL del profilo di avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-115">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="1ebd7-116">Binding per la porta 443 con un certificato assegnato.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-116">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="1ebd7-117">Ad esempio, il **nome host** per un sito Web aggiunto è impostato su "localhost" (anche il profilo di avvio userà "localhost" più avanti in questo argomento).</span><span class="sxs-lookup"><span data-stu-id="1ebd7-117">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="1ebd7-118">La porta è impostata su "443" (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="1ebd7-118">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="1ebd7-119">Al sito Web viene assegnato il **certificato di sviluppo di IIS Express**, ma può essere usato qualsiasi certificato valido:</span><span class="sxs-lookup"><span data-stu-id="1ebd7-119">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![Aggiungere la finestra del sito Web in IIS, in cui è visualizzato il binding impostato per localhost sulla porta 443 con un certificato assegnato.](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="1ebd7-121">Se l'installazione di IIS dispone già di un **sito Web predefinito** con un nome host che corrisponde al nome host dell'URL del profilo di avvio dell'app:</span><span class="sxs-lookup"><span data-stu-id="1ebd7-121">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="1ebd7-122">Aggiungere un binding per la porta 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="1ebd7-122">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="1ebd7-123">Assegnare un certificato valido al sito Web.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-123">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="1ebd7-124">Abilitare il supporto di IIS in fase di sviluppo in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1ebd7-124">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="1ebd7-125">Avviare il programma di installazione di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-125">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="1ebd7-126">Selezionare il componente **Supporto IIS in fase di sviluppo**.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-126">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="1ebd7-127">Il componente è elencato come facoltativo nel pannello **Riepilogo** del carico di lavoro **Sviluppo ASP.NET e Web**.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-127">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="1ebd7-128">Il componente installa il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), un modulo IIS nativo necessario per l'esecuzione delle app ASP.NET Core dietro a IIS in una configurazione di proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-128">The component installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps behind IIS in a reverse proxy configuration.</span></span>

![Modifica delle funzionalità di Visual Studio: la scheda Carichi di lavoro è selezionata.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="1ebd7-132">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="1ebd7-132">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="1ebd7-133">Reindirizzamento HTTPS</span><span class="sxs-lookup"><span data-stu-id="1ebd7-133">HTTPS redirection</span></span>

<span data-ttu-id="1ebd7-134">Per un nuovo progetto, selezionare la casella di controllo **Configura per HTTPS** nella finestra **Nuova applicazione Web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="1ebd7-134">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![Finestra Nuova applicazione Web ASP.NET Core con la casella di controllo Configura per HTTPS selezionata.](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="1ebd7-136">In un progetto esistente, usare il middleware di reindirizzamento HTTPS in `Startup.Configure` chiamando il metodo di estensione [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection):</span><span class="sxs-lookup"><span data-stu-id="1ebd7-136">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

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

### <a name="iis-launch-profile"></a><span data-ttu-id="1ebd7-137">Profilo di avvio IIS</span><span class="sxs-lookup"><span data-stu-id="1ebd7-137">IIS launch profile</span></span>

<span data-ttu-id="1ebd7-138">Creare un nuovo profilo di avvio per aggiungere il supporto IIS in fase di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="1ebd7-138">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="1ebd7-139">Per **Profilo** selezionare il pulsante **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-139">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="1ebd7-140">Denominare il profilo "IIS" nella finestra popup.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-140">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="1ebd7-141">Selezionare **OK** per creare il profilo.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-141">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="1ebd7-142">Per l'impostazione **Avvio** selezionare **IIS** dall'elenco.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-142">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="1ebd7-143">Selezionare la casella di controllo **Avvia browser** e specificare l'URL dell'endpoint.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-143">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="1ebd7-144">Usare il protocollo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-144">Use the HTTPS protocol.</span></span> <span data-ttu-id="1ebd7-145">In questo esempio viene usato `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-145">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="1ebd7-146">Nella sezione **Variabili di ambiente** selezionare il pulsante **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-146">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="1ebd7-147">Specificare una variabile di ambiente con una chiave `ASPNETCORE_ENVIRONMENT` e il valore `Development`.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-147">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="1ebd7-148">Nell'area **Impostazioni server Web** impostare **URL App**.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-148">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="1ebd7-149">In questo esempio viene usato `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="1ebd7-150">Salvare il profilo.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-150">Save the profile.</span></span>

![Finestra delle proprietà del progetto con la scheda Debug selezionata.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="1ebd7-155">In alternativa, aggiungere manualmente un profilo di avvio al file [launchSettings.json](http://json.schemastore.org/launchsettings) nell'app:</span><span class="sxs-lookup"><span data-stu-id="1ebd7-155">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

## <a name="run-the-project"></a><span data-ttu-id="1ebd7-156">Eseguire il progetto</span><span class="sxs-lookup"><span data-stu-id="1ebd7-156">Run the project</span></span>

<span data-ttu-id="1ebd7-157">Nell'interfaccia utente di Visual Studio impostare il pulsante Esegui sul profilo **IIS** e selezionare il pulsante per avviare l'app:</span><span class="sxs-lookup"><span data-stu-id="1ebd7-157">In the VS UI, set the Run button to the **IIS** profile and select the button to start the app:</span></span>

![Pulsante Esegui sulla barra degli strumenti di Visual Studio impostato sul profilo "IIS".](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="1ebd7-159">Visual Studio potrebbe richiedere un riavvio, se non si è in modalità amministratore.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-159">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="1ebd7-160">In tal caso riavviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-160">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="1ebd7-161">Se viene usato un certificato di sviluppo non attendibile, il browser potrebbe richiedere di creare un'eccezione per il certificato non attendibile.</span><span class="sxs-lookup"><span data-stu-id="1ebd7-161">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1ebd7-162">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="1ebd7-162">Additional resources</span></span>

* [<span data-ttu-id="1ebd7-163">Host ASP.NET Core in Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="1ebd7-163">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="1ebd7-164">Introduzione al modulo di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ebd7-164">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="1ebd7-165">Guida di riferimento per la configurazione del modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ebd7-165">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="1ebd7-166">Imporre HTTPS</span><span class="sxs-lookup"><span data-stu-id="1ebd7-166">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
