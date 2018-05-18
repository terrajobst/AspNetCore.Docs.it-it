---
title: Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core
author: shirhatti
description: Individuare il supporto per il debug di applicazioni ASP.NET Core durante l'esecuzione di base IIS in Windows Server.
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
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/17/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a><span data-ttu-id="c6869-103">Supporto di IIS in fase di sviluppo in Visual Studio per ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c6869-103">Development-time IIS support in Visual Studio for ASP.NET Core</span></span>

<span data-ttu-id="c6869-104">Dal [Sourabh Shirhatti](https://twitter.com/sshirhatti) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c6869-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c6869-105">Questo articolo descrive [Visual Studio](https://www.visualstudio.com/vs/) supporto per il debug di App di ASP.NET Core in esecuzione dietro IIS su Windows Server.</span><span class="sxs-lookup"><span data-stu-id="c6869-105">This article describes [Visual Studio](https://www.visualstudio.com/vs/) support for debugging ASP.NET Core apps running behind IIS on Windows Server.</span></span> <span data-ttu-id="c6869-106">Questo argomento vengono illustrati l'abilitazione di questa funzionalità e l'impostazione di un progetto.</span><span class="sxs-lookup"><span data-stu-id="c6869-106">This topic walks through enabling this feature and setting up a project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c6869-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="c6869-107">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a><span data-ttu-id="c6869-108">Abilitare IIS</span><span class="sxs-lookup"><span data-stu-id="c6869-108">Enable IIS</span></span>

1. <span data-ttu-id="c6869-109">Passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo).</span><span class="sxs-lookup"><span data-stu-id="c6869-109">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="c6869-110">Selezionare il **Internet Information Services** casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="c6869-110">Select the **Internet Information Services** check box.</span></span>

![Casella di controllo di Internet Information Services con le funzionalità di Windows archiviata come un quadrato bianco (non un segno di spunta) che indica che alcune delle funzionalità di IIS siano abilitate](development-time-iis-support/_static/enable_iis.png)

<span data-ttu-id="c6869-112">L'installazione di IIS potrebbe richiedere un riavvio del sistema.</span><span class="sxs-lookup"><span data-stu-id="c6869-112">The IIS installation may require a system restart.</span></span>

## <a name="configure-iis"></a><span data-ttu-id="c6869-113">Configura IIS</span><span class="sxs-lookup"><span data-stu-id="c6869-113">Configure IIS</span></span>

<span data-ttu-id="c6869-114">IIS deve avere un sito Web configurato con gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c6869-114">IIS must have a website configured with the following:</span></span>

* <span data-ttu-id="c6869-115">Nome host che corrisponde al nome host dell'URL profilo avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="c6869-115">A host name that matches the app's launch profile URL host name.</span></span>
* <span data-ttu-id="c6869-116">Associazione per la porta 443 con un certificato assegnati.</span><span class="sxs-lookup"><span data-stu-id="c6869-116">Binding for port 443 with an assigned certificate.</span></span>

<span data-ttu-id="c6869-117">Ad esempio, il **nome Host** per un sito Web aggiunto è impostato su "localhost" (il profilo di avvio utilizzeranno "localhost" più avanti in questo argomento).</span><span class="sxs-lookup"><span data-stu-id="c6869-117">For example, the **Host name** for an added website is set to "localhost" (the launch profile will also use "localhost" later in this topic).</span></span> <span data-ttu-id="c6869-118">La porta è impostata su "443" (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="c6869-118">The port is set to "443" (HTTPS).</span></span> <span data-ttu-id="c6869-119">Il **IIS Express sviluppo certificato** viene assegnato per il sito Web, ma qualsiasi certificato valido works:</span><span class="sxs-lookup"><span data-stu-id="c6869-119">The **IIS Express Development Certificate** is assigned to the website, but any valid certificate works:</span></span>

![Aggiungi finestra del sito Web in IIS che mostra il set di binding per localhost sulla porta 443 con il certificato assegnato.](development-time-iis-support/_static/add-website-window.png)

<span data-ttu-id="c6869-121">Se l'installazione di IIS già è un **sito Web predefinito** con un nome host che corrisponde al nome host dell'URL profilo avvio dell'app:</span><span class="sxs-lookup"><span data-stu-id="c6869-121">If the IIS installation already has a **Default Web Site** with a host name that matches the app's launch profile URL host name:</span></span>

* <span data-ttu-id="c6869-122">Aggiungere un binding di porta per la porta 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="c6869-122">Add a port binding for port 443 (HTTPS).</span></span>
* <span data-ttu-id="c6869-123">Assegnare un certificato valido per il sito Web.</span><span class="sxs-lookup"><span data-stu-id="c6869-123">Assign a valid certificate to the website.</span></span>

## <a name="enable-development-time-iis-support-in-visual-studio"></a><span data-ttu-id="c6869-124">Abilitare il supporto IIS in fase di sviluppo in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c6869-124">Enable development-time IIS support in Visual Studio</span></span>

1. <span data-ttu-id="c6869-125">Avviare il programma di installazione di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c6869-125">Launch the Visual Studio installer.</span></span>
1. <span data-ttu-id="c6869-126">Selezionare il **fase di sviluppo IIS supporta** componente.</span><span class="sxs-lookup"><span data-stu-id="c6869-126">Select the **Development time IIS support** component.</span></span> <span data-ttu-id="c6869-127">Il componente è elencato come facoltativi nel **riepilogo** pannello per il **sviluppo web ASP.NET e** carico di lavoro.</span><span class="sxs-lookup"><span data-stu-id="c6869-127">The component is listed as optional in the **Summary** panel for the **ASP.NET and web development** workload.</span></span> <span data-ttu-id="c6869-128">Il componente viene installato il [ASP.NET Core modulo](xref:fundamentals/servers/aspnet-core-module), ovvero un modulo nativo di IIS necessario per eseguire applicazioni dietro IIS ASP.NET Core in una configurazione di proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="c6869-128">The component installs the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module), which is a native IIS module required to run ASP.NET Core apps behind IIS in a reverse proxy configuration.</span></span>

![Modifica delle funzionalità di Visual Studio: la scheda Carichi di lavoro è selezionata.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a><span data-ttu-id="c6869-132">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="c6869-132">Configure the project</span></span>

### <a name="https-redirection"></a><span data-ttu-id="c6869-133">Reindirizzamento HTTPS</span><span class="sxs-lookup"><span data-stu-id="c6869-133">HTTPS redirection</span></span>

<span data-ttu-id="c6869-134">Per un nuovo progetto, selezionare la casella di controllo **Configura per HTTPS** nel **nuova applicazione Web di ASP.NET Core** finestra:</span><span class="sxs-lookup"><span data-stu-id="c6869-134">For a new project, select the check box to **Configure for HTTPS** in the **New ASP.NET Core Web Application** window:</span></span>

![Nuova finestra dell'applicazione Web di ASP.NET Core con la configurazione per la casella di controllo HTTPS.](development-time-iis-support/_static/new-app.png)

<span data-ttu-id="c6869-136">In un progetto esistente, utilizzare HTTPS reindirizzamento Middleware `Startup.Configure` chiamando il [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) metodo di estensione:</span><span class="sxs-lookup"><span data-stu-id="c6869-136">In an existing project, use HTTPS Redirection Middleware in `Startup.Configure` by calling the [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) extension method:</span></span>

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

### <a name="iis-launch-profile"></a><span data-ttu-id="c6869-137">IIS avviare profilo</span><span class="sxs-lookup"><span data-stu-id="c6869-137">IIS launch profile</span></span>

<span data-ttu-id="c6869-138">Creare un nuovo profilo di avvio per aggiungere il supporto IIS in fase di sviluppo:</span><span class="sxs-lookup"><span data-stu-id="c6869-138">Create a new launch profile to add development-time IIS support:</span></span>

1. <span data-ttu-id="c6869-139">Per **profilo**, selezionare il **New** pulsante.</span><span class="sxs-lookup"><span data-stu-id="c6869-139">For **Profile**, select the **New** button.</span></span> <span data-ttu-id="c6869-140">Denominare il profilo "IIS" nella finestra popup.</span><span class="sxs-lookup"><span data-stu-id="c6869-140">Name the profile "IIS" in the popup window.</span></span> <span data-ttu-id="c6869-141">Selezionare **OK** per creare il profilo.</span><span class="sxs-lookup"><span data-stu-id="c6869-141">Select **OK** to create the profile.</span></span>
1. <span data-ttu-id="c6869-142">Per il **avviare** impostazione, selezionare **IIS** dall'elenco.</span><span class="sxs-lookup"><span data-stu-id="c6869-142">For the **Launch** setting, select **IIS** from the list.</span></span>
1. <span data-ttu-id="c6869-143">Selezionare la casella di controllo **avvio browser** e fornire l'URL dell'endpoint.</span><span class="sxs-lookup"><span data-stu-id="c6869-143">Select the check box for **Launch browser** and provide the endpoint URL.</span></span> <span data-ttu-id="c6869-144">Utilizzare il protocollo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c6869-144">Use the HTTPS protocol.</span></span> <span data-ttu-id="c6869-145">In questo esempio viene usato `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="c6869-145">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="c6869-146">Nel **variabili di ambiente** sezione, selezionare il **Add** pulsante.</span><span class="sxs-lookup"><span data-stu-id="c6869-146">In the **Environment variables** section, select the **Add** button.</span></span> <span data-ttu-id="c6869-147">Fornire una variabile di ambiente con una chiave di `ASPNETCORE_ENVIRONMENT` e il valore `Development`.</span><span class="sxs-lookup"><span data-stu-id="c6869-147">Provide an environment variable with a key of `ASPNETCORE_ENVIRONMENT` and a value of `Development`.</span></span>
1. <span data-ttu-id="c6869-148">Nel **impostazioni del Server Web** area, impostare il **URL App**.</span><span class="sxs-lookup"><span data-stu-id="c6869-148">In the **Web Server Settings** area, set the **App URL**.</span></span> <span data-ttu-id="c6869-149">In questo esempio viene usato `https://localhost/WebApplication1`.</span><span class="sxs-lookup"><span data-stu-id="c6869-149">This example uses `https://localhost/WebApplication1`.</span></span>
1. <span data-ttu-id="c6869-150">Salvare il profilo.</span><span class="sxs-lookup"><span data-stu-id="c6869-150">Save the profile.</span></span>

![Finestra delle proprietà del progetto con la scheda Debug selezionata.](development-time-iis-support/_static/project_properties.png)

<span data-ttu-id="c6869-155">In alternativa, aggiungere manualmente un profilo di avvio per il [launchSettings.json](http://json.schemastore.org/launchsettings) file nell'app:</span><span class="sxs-lookup"><span data-stu-id="c6869-155">Alternatively, manually add a launch profile to the [launchSettings.json](http://json.schemastore.org/launchsettings) file in the app:</span></span>

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

## <a name="run-the-project"></a><span data-ttu-id="c6869-156">Eseguire il progetto</span><span class="sxs-lookup"><span data-stu-id="c6869-156">Run the project</span></span>

<span data-ttu-id="c6869-157">Nell'interfaccia utente di Visual Studio, impostare il pulsante di esecuzione il **IIS** del profilo e selezionare il pulsante per avviare l'app:</span><span class="sxs-lookup"><span data-stu-id="c6869-157">In the VS UI, set the Run button to the **IIS** profile and select the button to start the app:</span></span>

![Pulsante di esecuzione sulla barra degli strumenti di Visual Studio impostare il profilo "IIS".](development-time-iis-support/_static/toolbar.png)

<span data-ttu-id="c6869-159">Visual Studio potrebbe richiedere un riavvio, se non è in esecuzione come amministratore.</span><span class="sxs-lookup"><span data-stu-id="c6869-159">Visual Studio may prompt a restart if not running as an administrator.</span></span> <span data-ttu-id="c6869-160">In tal caso riavviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c6869-160">If prompted, restart Visual Studio.</span></span>

<span data-ttu-id="c6869-161">Se viene utilizzato un certificato di sviluppo non attendibile, il browser potrebbe essere necessario creare un'eccezione per il certificato non attendibile.</span><span class="sxs-lookup"><span data-stu-id="c6869-161">If an untrusted development certificate is used, the browser may require you to create an exception for the untrusted certificate.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c6869-162">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c6869-162">Additional resources</span></span>

* [<span data-ttu-id="c6869-163">Host ASP.NET Core in Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="c6869-163">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="c6869-164">Introduzione al modulo di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c6869-164">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="c6869-165">Guida di riferimento per la configurazione del modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c6869-165">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="c6869-166">Imporre HTTPS</span><span class="sxs-lookup"><span data-stu-id="c6869-166">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
