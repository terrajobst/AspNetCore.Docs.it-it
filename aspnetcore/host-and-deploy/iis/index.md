---
title: Host ASP.NET Core in Windows con IIS
author: guardrex
description: Informazioni su come ospitare app ASP.NET Core in Windows Server Internet Information Services (IIS).
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/index
ms.openlocfilehash: 18c7448ad79891d04eca1e939a0aeeabe417bde8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="5e305-103">Host ASP.NET Core in Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="5e305-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="5e305-104">Di [Luke Latham](https://github.com/guardrex) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5e305-104">By [Luke Latham](https://github.com/guardrex) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="supported-operating-systems"></a><span data-ttu-id="5e305-105">Sistemi operativi supportati</span><span class="sxs-lookup"><span data-stu-id="5e305-105">Supported operating systems</span></span>

<span data-ttu-id="5e305-106">Sono supportati i sistemi operativi seguenti:</span><span class="sxs-lookup"><span data-stu-id="5e305-106">The following operating systems are supported:</span></span>

* <span data-ttu-id="5e305-107">Windows 7 e versioni più recenti</span><span class="sxs-lookup"><span data-stu-id="5e305-107">Windows 7 and newer</span></span>
* <span data-ttu-id="5e305-108">Windows Server 2008 R2 e più recenti &#8224;</span><span class="sxs-lookup"><span data-stu-id="5e305-108">Windows Server 2008 R2 and newer&#8224;</span></span>

<span data-ttu-id="5e305-109">&#8224;A livello concettuale, la configurazione di IIS descritta in questo documento è valida anche per l'hosting delle app ASP.NET Core in Nano Server IIS. Per istruzioni specifiche vedere [ASP.NET Core con IIS in Nano Server](xref:tutorials/nano-server).</span><span class="sxs-lookup"><span data-stu-id="5e305-109">&#8224;Conceptually, the IIS configuration described in this document also applies to hosting ASP.NET Core apps on Nano Server IIS, but refer to [ASP.NET Core with IIS on Nano Server](xref:tutorials/nano-server) for specific instructions.</span></span>

<span data-ttu-id="5e305-110">Il [server HTTP.sys](xref:fundamentals/servers/httpsys) (chiamato in precedenza [WebListener](xref:fundamentals/servers/weblistener)) non funziona in una configurazione proxy inverso con IIS.</span><span class="sxs-lookup"><span data-stu-id="5e305-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) won't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="5e305-111">È necessario usare il [server Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="5e305-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="5e305-112">Configurazione di IIS</span><span class="sxs-lookup"><span data-stu-id="5e305-112">IIS configuration</span></span>

<span data-ttu-id="5e305-113">Abilitare il ruolo **Server Web (IIS)** e stabilire i servizi di ruolo.</span><span class="sxs-lookup"><span data-stu-id="5e305-113">Enable the **Web Server (IIS)** role and establish role services.</span></span>

### <a name="windows-desktop-operating-systems"></a><span data-ttu-id="5e305-114">Sistemi operativi desktop di Windows</span><span class="sxs-lookup"><span data-stu-id="5e305-114">Windows desktop operating systems</span></span>

<span data-ttu-id="5e305-115">Passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo).</span><span class="sxs-lookup"><span data-stu-id="5e305-115">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="5e305-116">Aprire il gruppo per **Internet Information Services** e **Strumenti di gestione Web**.</span><span class="sxs-lookup"><span data-stu-id="5e305-116">Open the group for **Internet Information Services** and **Web Management Tools**.</span></span> <span data-ttu-id="5e305-117">Selezionare la casella per **Console di gestione IIS**.</span><span class="sxs-lookup"><span data-stu-id="5e305-117">Check the box for **IIS Management Console**.</span></span> <span data-ttu-id="5e305-118">Selezionare la casella per **Servizi World Wide Web**.</span><span class="sxs-lookup"><span data-stu-id="5e305-118">Check the box for **World Wide Web Services**.</span></span> <span data-ttu-id="5e305-119">Accettare le funzionalità predefinite per **Servizi World Wide Web** o personalizzare le funzionalità IIS.</span><span class="sxs-lookup"><span data-stu-id="5e305-119">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

![La console di gestione IIS e servizi World Wide Web sono selezionati nelle funzionalità di Windows.](index/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a><span data-ttu-id="5e305-121">Sistemi operativi Windows Server</span><span class="sxs-lookup"><span data-stu-id="5e305-121">Windows Server operating systems</span></span>

<span data-ttu-id="5e305-122">Per i sistemi operativi del server, usare la procedura guidata **Aggiungi ruoli e funzionalità** tramite il menù **Gestisci** o il collegamento in **Server Manager**.</span><span class="sxs-lookup"><span data-stu-id="5e305-122">For server operating systems, use the **Add Roles and Features** wizard via the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="5e305-123">Nel passaggio **Ruoli del server** selezionare la casella per **Server Web (IIS)**.</span><span class="sxs-lookup"><span data-stu-id="5e305-123">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

![Il ruolo Server Web IIS viene selezionato nel passaggio dei ruoli del server Selezionare.](index/_static/server-roles-ws2016.png)

<span data-ttu-id="5e305-125">Nel passaggio **Servizi ruolo** selezionare i servizi ruolo IIS desiderati o accettare i servizi ruolo predefiniti specificati.</span><span class="sxs-lookup"><span data-stu-id="5e305-125">On the **Role services** step, select the IIS role services desired or accept the default role services provided.</span></span>

![I servizi ruolo predefiniti vengono selezionati nel passaggio Selezionare i servizi ruolo.](index/_static/role-services-ws2016.png)

<span data-ttu-id="5e305-127">Procedere con il passaggio **Conferma** per installare il ruolo del server web e i servizi.</span><span class="sxs-lookup"><span data-stu-id="5e305-127">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="5e305-128">Dopo l'installazione del ruolo Server Web (IIS) non è necessario riavviare il server o IIS.</span><span class="sxs-lookup"><span data-stu-id="5e305-128">A server/IIS restart isn't required after installing the Web Server (IIS) role.</span></span>

## <a name="install-the-net-core-windows-server-hosting-bundle"></a><span data-ttu-id="5e305-129">Installare l'aggregazione di Hosting di.NET Core Windows Server</span><span class="sxs-lookup"><span data-stu-id="5e305-129">Install the .NET Core Windows Server Hosting bundle</span></span>

1. <span data-ttu-id="5e305-130">Installare l'[aggregazione di Hosting di.NET Core Windows Server](https://aka.ms/dotnetcore-2-windowshosting) nel sistema di hosting.</span><span class="sxs-lookup"><span data-stu-id="5e305-130">Install the [.NET Core Windows Server Hosting bundle](https://aka.ms/dotnetcore-2-windowshosting) on the hosting system.</span></span> <span data-ttu-id="5e305-131">L'aggregazione installa il runtime di .NET Core, la libreria di .NET Core e il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="5e305-131">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="5e305-132">Il modulo crea il proxy inverso tra IIS e il server Kestrel.</span><span class="sxs-lookup"><span data-stu-id="5e305-132">The module creates the reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="5e305-133">Se nel sistema non è presente una connessione a Internet, ottenere e installare [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) prima di installare l'aggregazione di Hosting di .NET Core Windows Server.</span><span class="sxs-lookup"><span data-stu-id="5e305-133">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Windows Server Hosting bundle.</span></span>

2. <span data-ttu-id="5e305-134">Riavviare il sistema o eseguire **net stop was /y** seguito da **net start w3svc** da un prompt dei comandi per visualizzare una modifica al percorso di sistema.</span><span class="sxs-lookup"><span data-stu-id="5e305-134">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt to pick up a change to the system PATH.</span></span>

> [!NOTE]
> <span data-ttu-id="5e305-135">Per informazioni sulla configurazione condivisa di IIS, vedere [Modulo di ASP.NET Core con configurazione condivisa di IIS](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span><span class="sxs-lookup"><span data-stu-id="5e305-135">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="5e305-136">Installare la Distribuzione Web quando si pubblica con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e305-136">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="5e305-137">Quando si distribuiscono app ai server con [Distribuzione Web](/iis/publish/using-web-deploy/introduction-to-web-deploy), installare la versione più recente di Distribuzione Web nel server.</span><span class="sxs-lookup"><span data-stu-id="5e305-137">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="5e305-138">Per installare Distribuzione Web usare l'[Installazione guidata piattaforma Web (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) o ottenere direttamente un programma di installazione in [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span><span class="sxs-lookup"><span data-stu-id="5e305-138">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="5e305-139">Il metodo preferito consiste nell'usare WebPI.</span><span class="sxs-lookup"><span data-stu-id="5e305-139">The preferred method is to use WebPI.</span></span> <span data-ttu-id="5e305-140">WebPI offre un'installazione autonoma e una configurazione per i provider di hosting.</span><span class="sxs-lookup"><span data-stu-id="5e305-140">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="5e305-141">Configurazione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="5e305-141">Application configuration</span></span>

### <a name="enabling-the-iisintegration-components"></a><span data-ttu-id="5e305-142">Abilitare i componenti IISIntegration</span><span class="sxs-lookup"><span data-stu-id="5e305-142">Enabling the IISIntegration components</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5e305-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5e305-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5e305-144">Un file *Program.cs* tipico chiama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) per avviare la configurazione di un host.</span><span class="sxs-lookup"><span data-stu-id="5e305-144">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to begin setting up a host.</span></span> <span data-ttu-id="5e305-145">`CreateDefaultBuilder` configura [Kestrel](xref:fundamentals/servers/kestrel) come server Web e abilita l'integrazione IIS configurando il percorso base e la porta per il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module):</span><span class="sxs-lookup"><span data-stu-id="5e305-145">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5e305-146">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5e305-146">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5e305-147">Includere una dipendenza dal pacchetto [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) nelle dipendenze dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="5e305-147">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="5e305-148">Incorporare il middleware di integrazione IIS nell'app aggiungendo il metodo di estensione *UseIISIntegration* a *WebHostBuilder*:</span><span class="sxs-lookup"><span data-stu-id="5e305-148">Incorporate IIS Integration middleware into the app by adding the *UseIISIntegration* extension method to *WebHostBuilder*:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="5e305-149">Sono necessari sia `UseKestrel` sia `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="5e305-149">Both `UseKestrel` and `UseIISIntegration` are required.</span></span> <span data-ttu-id="5e305-150">Il fatto che il codice chiami `UseIISIntegration` non influisce sulla portabilità del codice stesso.</span><span class="sxs-lookup"><span data-stu-id="5e305-150">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="5e305-151">Se l'app non viene eseguita dietro IIS (ad esempio, se l'app viene eseguita direttamente in Kestrel), `UseIISIntegration` non esegue nessuna operazione.</span><span class="sxs-lookup"><span data-stu-id="5e305-151">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` no-ops.</span></span>

---

<span data-ttu-id="5e305-152">Per altre informazioni sull'hosting, vedere [Hosting in ASP.NET Core](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="5e305-152">For more information on hosting, see [Hosting in ASP.NET Core](xref:fundamentals/hosting).</span></span>

### <a name="iis-options"></a><span data-ttu-id="5e305-153">Opzioni IIS</span><span class="sxs-lookup"><span data-stu-id="5e305-153">IIS options</span></span>

<span data-ttu-id="5e305-154">Per configurare le opzioni di IIS includere una configurazione del servizio per `IISOptions` in `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5e305-154">To configure IIS options, include a service configuration for `IISOptions` in `ConfigureServices`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| <span data-ttu-id="5e305-155">Opzione</span><span class="sxs-lookup"><span data-stu-id="5e305-155">Option</span></span>                         | <span data-ttu-id="5e305-156">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="5e305-156">Default</span></span> | <span data-ttu-id="5e305-157">Impostazione</span><span class="sxs-lookup"><span data-stu-id="5e305-157">Setting</span></span> |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="5e305-158">Se è `true`, il middleware di autenticazione imposta `HttpContext.User` e risponde alle richieste generiche.</span><span class="sxs-lookup"><span data-stu-id="5e305-158">If `true`, the authentication middleware sets the `HttpContext.User` and responds to generic challenges.</span></span> <span data-ttu-id="5e305-159">Se è `false`, il middleware di autenticazione fornisce solo un'identità (`HttpContext.User`) e risponde alle richieste solo in base a un’istruzione esplicita di `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="5e305-159">If `false`, the authentication middleware only provides an identity (`HttpContext.User`) and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="5e305-160">Per il funzionamento di `AutomaticAuthentication` l’autenticazione di Windows deve essere abilitata in IIS.</span><span class="sxs-lookup"><span data-stu-id="5e305-160">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="5e305-161">Imposta il nome visualizzato dagli utenti nelle pagine di accesso.</span><span class="sxs-lookup"><span data-stu-id="5e305-161">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="5e305-162">Se è `true` ed è presente l’intestazione della richiesta `MS-ASPNETCORE-CLIENTCERT`, `HttpContext.Connection.ClientCertificate` viene popolato.</span><span class="sxs-lookup"><span data-stu-id="5e305-162">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="webconfig"></a><span data-ttu-id="5e305-163">web.config</span><span class="sxs-lookup"><span data-stu-id="5e305-163">web.config</span></span>

<span data-ttu-id="5e305-164">Lo scopo principale del file *web.config* è la configurazione del [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="5e305-164">The *web.config* file's primary purpose is to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="5e305-165">e può fornire facoltativamente altre impostazioni di configurazione di IIS.</span><span class="sxs-lookup"><span data-stu-id="5e305-165">It may optionally provide additional IIS configuration settings.</span></span> <span data-ttu-id="5e305-166">La creazione, la trasformazione e la pubblicazione di *web.config* vengono gestite da .NET Core Web SDK (`Microsoft.NET.Sdk.Web`),</span><span class="sxs-lookup"><span data-stu-id="5e305-166">Creating, transforming, and publishing *web.config* is handled by the .NET Core Web SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="5e305-167">impostato all'inizio del file di progetto `<Project Sdk="Microsoft.NET.Sdk.Web">`.</span><span class="sxs-lookup"><span data-stu-id="5e305-167">The SDK is set  at the top of the project file, `<Project Sdk="Microsoft.NET.Sdk.Web">`.</span></span> <span data-ttu-id="5e305-168">Per impedire che l'SDK trasformi il file *web.config*, aggiungere la proprietà **\<IsTransformWebConfigDisabled >** al file di progetto con un'impostazione di `true`:</span><span class="sxs-lookup"><span data-stu-id="5e305-168">To prevent the SDK from transforming the *web.config* file, add the **\<IsTransformWebConfigDisabled>** property to the project file with a setting of `true`:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="5e305-169">Se nel progetto è presente un file *web.config*, il file viene trasformato con il valore di *processPath* e gli *argomenti* corretti per la configurazione del [modulo di ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), quindi viene spostato nell'[output pubblicato](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="5e305-169">If a *web.config* file is in the project, it's transformed with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span> <span data-ttu-id="5e305-170">La trasformazione non modifica le impostazioni di configurazione di IIS incluse nel file.</span><span class="sxs-lookup"><span data-stu-id="5e305-170">The transformation doesn't modify IIS configuration settings in the file.</span></span>

### <a name="webconfig-location"></a><span data-ttu-id="5e305-171">Percorso di web.config</span><span class="sxs-lookup"><span data-stu-id="5e305-171">web.config location</span></span>

<span data-ttu-id="5e305-172">Le app .NET core sono ospitate tramite un proxy inverso tra IIS e il server Kestrel.</span><span class="sxs-lookup"><span data-stu-id="5e305-172">.NET Core apps are hosted via a reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="5e305-173">Per creare il proxy inverso, il file *web.config* deve essere presente nel percorso radice del contenuto (in genere il percorso base dell'app) dell'app distribuita, che è il percorso fisico del sito Web reso disponibile a IIS.</span><span class="sxs-lookup"><span data-stu-id="5e305-173">In order to create the reverse proxy, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app, which is the website physical path provided to IIS.</span></span> <span data-ttu-id="5e305-174">Il file *web.config* deve essere presente nella radice dell'app per abilitare la pubblicazione di più app mediante Distribuzione Web.</span><span class="sxs-lookup"><span data-stu-id="5e305-174">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="5e305-175">Nel percorso fisico dell'app sono presenti file riservati, incluse le sottocartelle quali *\<assembly_name>.runtimeconfig.json*, *\<assembly_name>.xml* (commenti in formato documentazione XML) e *\<assembly_name>.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="5e305-175">Sensitive files exist on the app's physical path, including subfolders, such as *\<assembly_name>.runtimeconfig.json*, *\<assembly_name>.xml* (XML Documentation comments), and *\<assembly_name>.deps.json*.</span></span> <span data-ttu-id="5e305-176">Quando il file *web.config* è presente e configura il sito, IIS limita la disponibilità di questi file riservati.</span><span class="sxs-lookup"><span data-stu-id="5e305-176">When the *web.config* file is present and configures the site, IIS prevents these sensitive files from being served.</span></span> <span data-ttu-id="5e305-177">**Di conseguenza è importante che il file *web.config* non sia inavvertitamente rinominato o rimosso dalla distribuzione.**</span><span class="sxs-lookup"><span data-stu-id="5e305-177">**Therefore, it's important that the *web.config* file isn't accidently renamed or removed from the deployment.**</span></span>

## <a name="create-the-iis-website"></a><span data-ttu-id="5e305-178">Creare il sito Web IIS</span><span class="sxs-lookup"><span data-stu-id="5e305-178">Create the IIS Website</span></span>

1. <span data-ttu-id="5e305-179">Nel sistema IIS di destinazione creare una cartella per contenere le cartelle e i file pubblicati dell'app, descritti in [Struttura della directory](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="5e305-179">On the target IIS system, create a folder to contain the app's published folders and files, which are described in [Directory Structure](xref:host-and-deploy/directory-structure).</span></span>

2. <span data-ttu-id="5e305-180">All'interno della cartella creare una cartella *logs* per contenere i log stdout quando è abilitata la registrazione stdout.</span><span class="sxs-lookup"><span data-stu-id="5e305-180">Within the folder, create a *logs* folder to hold stdout logs when stdout logging is enabled.</span></span> <span data-ttu-id="5e305-181">Se l'app viene distribuita con una cartella *logs* nel payload, ignorare questo passaggio.</span><span class="sxs-lookup"><span data-stu-id="5e305-181">If the app is deployed with a *logs* folder in the payload, skip this step.</span></span> <span data-ttu-id="5e305-182">Per istruzioni su come impostare MSBuild per la creazione della cartella *logs*, vedere l'argomento [Struttura di directory](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="5e305-182">For instructions on how to make MSBuild create the *logs* folder, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>

3. <span data-ttu-id="5e305-183">In **Gestione IIS**, creare un nuovo sito Web.</span><span class="sxs-lookup"><span data-stu-id="5e305-183">In **IIS Manager**, create a new website.</span></span> <span data-ttu-id="5e305-184">Specificare un **Nome del sito** e impostare il **Percorso fisico** per la cartella di distribuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="5e305-184">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="5e305-185">Fornire la configurazione dell'**Associazione** e creare il sito Web.</span><span class="sxs-lookup"><span data-stu-id="5e305-185">Provide the **Binding** configuration and create the website.</span></span>

4. <span data-ttu-id="5e305-186">Impostare il pool di applicazioni su **Nessun codice gestito**.</span><span class="sxs-lookup"><span data-stu-id="5e305-186">Set the application pool to **No Managed Code**.</span></span> <span data-ttu-id="5e305-187">ASP.NET Core viene eseguito in un processo separato e gestisce il runtime.</span><span class="sxs-lookup"><span data-stu-id="5e305-187">ASP.NET Core runs in a separate process and manages the runtime.</span></span>

5. <span data-ttu-id="5e305-188">Aprire la finestra **Aggiungi sito Web**.</span><span class="sxs-lookup"><span data-stu-id="5e305-188">Open the **Add Website** window.</span></span>

   ![Selezionare "Aggiungi sito Web" dal menu di scelta rapida dei siti.](index/_static/add-website-context-menu-ws2016.png)

6. <span data-ttu-id="5e305-190">Configurare il sito Web.</span><span class="sxs-lookup"><span data-stu-id="5e305-190">Configure the website.</span></span>

   ![Specificare il nome del sito, il percorso fisico e il nome Host nel passaggio Aggiungi sito Web.](index/_static/add-website-ws2016.png)

7. <span data-ttu-id="5e305-192">Nel riquadro **Pool di applicazioni** aprire la finestra **Modifica Pool di applicazioni** facendo clic con il pulsante destro del mouse sul pool di app del sito Web e selezionando **Impostazioni Basic** dal menu a comparsa.</span><span class="sxs-lookup"><span data-stu-id="5e305-192">In the **Application Pools** panel, open the **Edit Application Pool** window by right-clicking on the website's app pool and selecting **Basic Settings...** from the popup menu.</span></span>

   ![Selezionare le Impostazioni Basic dal menu di scelta rapida del Pool di applicazioni.](index/_static/apppools-basic-settings-ws2016.png)

8. <span data-ttu-id="5e305-194">Impostare la **versione CLR.NET** su **Nessun codice gestito**.</span><span class="sxs-lookup"><span data-stu-id="5e305-194">Set the **.NET CLR version** to **No Managed Code**.</span></span>

   ![Non impostare il Codice gestito per la versione CLR.NET.](index/_static/edit-apppool-ws2016.png)
     
    <span data-ttu-id="5e305-196">Nota: l'impostazione della **versione CLR.NET** su **Nessun codice gestito** è facoltativa.</span><span class="sxs-lookup"><span data-stu-id="5e305-196">Note: Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span> <span data-ttu-id="5e305-197">ASP.NET Core non si basa sul caricamento del CLR del desktop.</span><span class="sxs-lookup"><span data-stu-id="5e305-197">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span>

9. <span data-ttu-id="5e305-198">Confermare che l'identità del modello del processo disponga delle autorizzazioni appropriate.</span><span class="sxs-lookup"><span data-stu-id="5e305-198">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="5e305-199">Se l'identità predefinita del pool di app (**Modello di processo** > **Identità**) viene modificata da **ApplicationPoolIdentity** a un'altra identità, verificare che la nuova identità disponga delle autorizzazioni necessarie per l'accesso alla cartella dell'app, al database e alle altre risorse richieste.</span><span class="sxs-lookup"><span data-stu-id="5e305-199">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span>
   
## <a name="deploy-the-app"></a><span data-ttu-id="5e305-200">Distribuire l'app</span><span class="sxs-lookup"><span data-stu-id="5e305-200">Deploy the app</span></span>

<span data-ttu-id="5e305-201">Distribuire l'app nella cartella creata nel sistema IIS di destinazione.</span><span class="sxs-lookup"><span data-stu-id="5e305-201">Deploy the app to the folder created on the target IIS system.</span></span> <span data-ttu-id="5e305-202">Il metodo consigliato per la distribuzione è [Distribuzione Web](/iis/publish/using-web-deploy/introduction-to-web-deploy).</span><span class="sxs-lookup"><span data-stu-id="5e305-202">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

<span data-ttu-id="5e305-203">Verificare che l'app pubblicata per la distribuzione non sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="5e305-203">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="5e305-204">I file nella cartella *publish* sono bloccati durante l'esecuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="5e305-204">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="5e305-205">I file bloccati non possono essere sovrascritti.</span><span class="sxs-lookup"><span data-stu-id="5e305-205">Locked files can't be overwritten.</span></span> <span data-ttu-id="5e305-206">Per rilasciare i file bloccati in una distribuzione, arrestare il pool di applicazioni:</span><span class="sxs-lookup"><span data-stu-id="5e305-206">To release locked files in a deployment, stop the app pool:</span></span>

* <span data-ttu-id="5e305-207">Manualmente in Gestione IIS sul server.</span><span class="sxs-lookup"><span data-stu-id="5e305-207">Manually in the IIS Manager on the server.</span></span>
* <span data-ttu-id="5e305-208">Usando Distribuzione Web e facendo riferimento a `Microsoft.NET.Sdk.Web` nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="5e305-208">Using Web Deploy and referencing `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="5e305-209">Nella radice della directory dell'app web viene posizionato un file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="5e305-209">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="5e305-210">Quando il file è presente, il modulo ASP.NET Core arresta normalmente l'app e rende disponibile il file *app_offline.htm* durante la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="5e305-210">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="5e305-211">Per altre informazioni, vedere la [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span><span class="sxs-lookup"><span data-stu-id="5e305-211">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>
* <span data-ttu-id="5e305-212">Usare PowerShell per arrestare e riavviare il pool di applicazioni (richiede PowerShell 5 o versione successiva):</span><span class="sxs-lookup"><span data-stu-id="5e305-212">Use PowerShell to stop and restart the app pool (requires PowerShell 5 or later):</span></span>
  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="5e305-213">Distribuzione web con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e305-213">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="5e305-214">Vedere l'argomento [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) (Profili di pubblicazione Visual Studio per la distribuzione di app ASP.NET Core) per informazioni su come creare un profilo di pubblicazione da usare con Distribuzione Web.</span><span class="sxs-lookup"><span data-stu-id="5e305-214">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="5e305-215">Se il provider di hosting rende disponibile un Profilo di pubblicazione o il supporto per crearne uno, scaricare il profilo e importarlo usando la finestra di dialogo **Pubblica** di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5e305-215">If the hosting provider supplies a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![Pagina della finestra di dialogo Pubblica](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="5e305-217">Distribuzione Web all'esterno di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e305-217">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="5e305-218">È anche possibile usare [Distribuzione Web](/iis/publish/using-web-deploy/introduction-to-web-deploy) all'esterno di Visual Studio dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="5e305-218">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="5e305-219">Per altre informazioni sulla distribuzione, vedere [Strumento Distribuzione Web](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span><span class="sxs-lookup"><span data-stu-id="5e305-219">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="5e305-220">Alternative a Distribuzione web</span><span class="sxs-lookup"><span data-stu-id="5e305-220">Alternatives to Web Deploy</span></span>

<span data-ttu-id="5e305-221">Usare uno dei metodi disponibili, ad esempio Xcopy, Robocopy o PowerShell per spostare l'app nel sistema host.</span><span class="sxs-lookup"><span data-stu-id="5e305-221">Use any of several methods to move the app to the hosting system, such as Xcopy, Robocopy, or PowerShell.</span></span> <span data-ttu-id="5e305-222">Gli utenti di Visual Studio possono usare gli [Esempi di pubblicazione](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).</span><span class="sxs-lookup"><span data-stu-id="5e305-222">Visual Studio users may use the [Publish Samples](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="5e305-223">Esplorare il sito Web</span><span class="sxs-lookup"><span data-stu-id="5e305-223">Browse the website</span></span>

![Il browser Microsoft Edge ha caricato la pagina di avvio IIS.](index/_static/browsewebsite.png)

## <a name="data-protection"></a><span data-ttu-id="5e305-225">Protezione dei dati</span><span class="sxs-lookup"><span data-stu-id="5e305-225">Data protection</span></span>

<span data-ttu-id="5e305-226">La protezione dei dati viene usata da diversi middlewares di ASP.NET, inclusi quelli usati nell'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="5e305-226">Data Protection is used by several ASP.NET middlewares, including those used in authentication.</span></span> <span data-ttu-id="5e305-227">Anche se le DPAPI (Data Protection API) non vengono chiamate dal codice dell'utente, è consigliabile configurare Protezione dati per la creazione di un archivio chiavi permanente con uno script di distribuzione o nel codice dell'utente.</span><span class="sxs-lookup"><span data-stu-id="5e305-227">Even if Data Protection APIs aren't called from user's code, Data Protection should be configured with a deployment script or in user code to create a persistent key store.</span></span> <span data-ttu-id="5e305-228">Se non si configura la protezione dei dati, le chiavi vengono mantenute in memoria ed eliminate al riavvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="5e305-228">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="5e305-229">Se il gruppo di chiavi viene archiviato in memoria, quando l'app viene riavviata:</span><span class="sxs-lookup"><span data-stu-id="5e305-229">If the keyring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="5e305-230">Tutti i token di autenticazione dei moduli non sono più validi.</span><span class="sxs-lookup"><span data-stu-id="5e305-230">All forms authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="5e305-231">Gli utenti devono ripetere l'accesso alla richiesta successiva.</span><span class="sxs-lookup"><span data-stu-id="5e305-231">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="5e305-232">Tutti i dati protetti con il gruppo di chiavi non possono più essere decrittografati.</span><span class="sxs-lookup"><span data-stu-id="5e305-232">Any data protected with the keyring can no longer be decrypted.</span></span>

<span data-ttu-id="5e305-233">Per configurare la protezione dei dati in IIS è necessario usare **uno** degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="5e305-233">To configure Data Protection under IIS, use **one** of the following approaches:</span></span>

### <a name="create-a-data-protection-registry-hive"></a><span data-ttu-id="5e305-234">Creare un Hive del Registro di sistema di protezione dei dati</span><span class="sxs-lookup"><span data-stu-id="5e305-234">Create a Data Protection Registry Hive</span></span>

<span data-ttu-id="5e305-235">Le chiavi di protezione dei dati usate dalle app ASP.NET vengono archiviate in hive del registro esterni alle app.</span><span class="sxs-lookup"><span data-stu-id="5e305-235">Data Protection keys used by ASP.NET apps are stored in registry hives external to the apps.</span></span> <span data-ttu-id="5e305-236">Per mantenere le chiavi per una determinata app, creare un hive del registro per il pool dell'app.</span><span class="sxs-lookup"><span data-stu-id="5e305-236">To persist the keys for a given app, create a registry hive for the app pool.</span></span>

<span data-ttu-id="5e305-237">Per le installazioni IIS autonome non web farm è possibile usare lo [script PowerShell Data Protection Provision-AutoGenKeys.ps1 (Provisioning di protezione dati-AutoGenKeys.ps1)](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) per ogni pool di app usato con un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5e305-237">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="5e305-238">Questo script crea una chiave del Registro di sistema speciale nel registro HKLM che viene inclusa nell'elenco di controllo di accesso (ACL) solo per l'account del processo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="5e305-238">This script creates a special registry key in the HKLM registry that's ACLed only to the worker process account.</span></span> <span data-ttu-id="5e305-239">Le chiavi vengono crittografate a riposo tramite DPAPI con una chiave a livello del computer.</span><span class="sxs-lookup"><span data-stu-id="5e305-239">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

<span data-ttu-id="5e305-240">In scenari di web farm un'app può essere configurata in modo da usare un percorso UNC per archiviare il relativo gruppo di chiavi di protezione dei dati.</span><span class="sxs-lookup"><span data-stu-id="5e305-240">In web farm scenarios, an app can be configured to use a UNC path to store its data protection keyring.</span></span> <span data-ttu-id="5e305-241">Per impostazione predefinita, le chiavi di protezione dei dati non vengono crittografate.</span><span class="sxs-lookup"><span data-stu-id="5e305-241">By default, the data protection keys are not encrypted.</span></span> <span data-ttu-id="5e305-242">Assicurarsi che le autorizzazioni file per una condivisione di questo tipo siano limitate all'app che viene eseguita come account di Windows.</span><span class="sxs-lookup"><span data-stu-id="5e305-242">Ensure that the file permissions for such a share are limited to the Windows account the app runs as.</span></span> <span data-ttu-id="5e305-243">È anche possibile usare un certificato X509 per la protezione delle chiavi a riposo.</span><span class="sxs-lookup"><span data-stu-id="5e305-243">In addition, an X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="5e305-244">Considerare l'implementazione di un meccanismo per consentire agli utenti di caricare i certificati: inserire i certificati nell'archivio certificati attendibili dell'utente e assicurarsi che siano disponibili in tutti i computer in cui viene eseguita l'app dell'utente.</span><span class="sxs-lookup"><span data-stu-id="5e305-244">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="5e305-245">Vedere [Configurazione della protezione dati](xref:security/data-protection/configuration/overview) per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="5e305-245">See [Configuring Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

### <a name="configure-the-iis-application-pool-to-load-the-user-profile"></a><span data-ttu-id="5e305-246">Configurare il Pool di applicazioni IIS per caricare il profilo utente</span><span class="sxs-lookup"><span data-stu-id="5e305-246">Configure the IIS Application Pool to load the user profile</span></span>

<span data-ttu-id="5e305-247">Questa impostazione si trova nella sezione **Modello del processo** in **Impostazioni avanzate** per il pool di app.</span><span class="sxs-lookup"><span data-stu-id="5e305-247">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="5e305-248">Impostare Carica profilo utente su `True`.</span><span class="sxs-lookup"><span data-stu-id="5e305-248">Set Load User Profile to `True`.</span></span> <span data-ttu-id="5e305-249">Questa operazione archivia le chiavi nella directory del profilo utente e le mantiene protette tramite DPAPI con una chiave specifica per l'account utente usato per il pool di app.</span><span class="sxs-lookup"><span data-stu-id="5e305-249">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used for the app pool.</span></span>

### <a name="use-the-file-system-as-a-key-ring-store"></a><span data-ttu-id="5e305-250">Usare il file system come archivio del gruppo di chiavi</span><span class="sxs-lookup"><span data-stu-id="5e305-250">Use the file system as a key ring store</span></span>

<span data-ttu-id="5e305-251">Modificare il codice dell'app per [usare il file system come archivio del gruppo di chiavi](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="5e305-251">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="5e305-252">Usare un certificato X509 per proteggere le chiavi e verificare che sia un certificato attendibile.</span><span class="sxs-lookup"><span data-stu-id="5e305-252">Use an X509 certificate to protect the keyring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="5e305-253">Se si tratta di un certificato autofirmato, inserirlo nell'archivio radice attendibile.</span><span class="sxs-lookup"><span data-stu-id="5e305-253">If it's a self signed certificate, place the certificate in the Trusted Root store.</span></span>

<span data-ttu-id="5e305-254">Quando si usa IIS in una web farm:</span><span class="sxs-lookup"><span data-stu-id="5e305-254">When using IIS in a web farm:</span></span>

* <span data-ttu-id="5e305-255">Usare una condivisione di file a cui possono accedere tutti i computer.</span><span class="sxs-lookup"><span data-stu-id="5e305-255">Use a file share that all machines can access.</span></span>
* <span data-ttu-id="5e305-256">Distribuire un certificato X509 in ogni computer.</span><span class="sxs-lookup"><span data-stu-id="5e305-256">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="5e305-257">Configurare [protezione dei dati nel codice](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="5e305-257">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

### <a name="set-a-machine-wide-policy-for-data-protection"></a><span data-ttu-id="5e305-258">Impostare criteri a livello di computer per la protezione dei dati</span><span class="sxs-lookup"><span data-stu-id="5e305-258">Set a machine-wide policy for data protection</span></span>

<span data-ttu-id="5e305-259">Il sistema di protezione dei dati ha un supporto limitato per l'impostazione di [criteri a livello di computer](xref:security/data-protection/configuration/machine-wide-policy) predefiniti per tutte le app che usano le API di protezione dei dati.</span><span class="sxs-lookup"><span data-stu-id="5e305-259">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="5e305-260">Vedere la documentazione sulla [protezione dei dati](xref:security/data-protection/index) per maggiori dettagli.</span><span class="sxs-lookup"><span data-stu-id="5e305-260">See the [data protection](xref:security/data-protection/index) documentation for more details.</span></span>

## <a name="configuration-of-sub-applications"></a><span data-ttu-id="5e305-261">Configurazione delle sotto-applicazioni</span><span class="sxs-lookup"><span data-stu-id="5e305-261">Configuration of sub-applications</span></span>

<span data-ttu-id="5e305-262">Le sotto-app aggiunte sotto l'app radice non devono includere il modulo ASP.NET Core come gestore.</span><span class="sxs-lookup"><span data-stu-id="5e305-262">Sub-apps added under the root app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="5e305-263">Se il modulo viene aggiunto come gestore nel file *web.config* di una sotto-app, quando si prova a esplorare la sotto-app si riceve un messaggio 500.19 (Errore interno del server) che segnala errori nel file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="5e305-263">If the module is added as a handler in a sub-app's *web.config* file, a 500.19 (Internal Server Error) referencing the faulty config file is received when attempting to browse the sub-app.</span></span> <span data-ttu-id="5e305-264">Nell'esempio seguente viene illustrato il contenuto di un file *web.config* pubblicato per una sotto-app di ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="5e305-264">The following example shows the contents of a published *web.config* file for an ASP.NET Core sub-app:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="5e305-265">Se si ospita una sotto-app non ASP.NET Core in un'app ASP.NET Core, è necessario rimuovere in modo esplicito il gestore ereditato nel file *web.config* della sotto-app:</span><span class="sxs-lookup"><span data-stu-id="5e305-265">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="5e305-266">Per altre informazioni sulla configurazione del modulo di ASP.NET Core, vedere l'argomento [Introduzione al modulo di ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) e la [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="5e305-266">For more information on configuring the ASP.NET Core Module, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="5e305-267">Configurazione di IIS con web.config</span><span class="sxs-lookup"><span data-stu-id="5e305-267">Configuration of IIS with web.config</span></span>

<span data-ttu-id="5e305-268">La configurazione di IIS è influenzata dalla sezione **\<system.webServer>** di *web.config* per le funzionalità IIS valide per una configurazione proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="5e305-268">IIS configuration is influenced by the **\<system.webServer>** section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="5e305-269">Se IIS è configurato a livello di sistema per l'uso della compressione dinamica, tale impostazione può essere disattivata per un'app mediante l'elemento **\<urlCompression>** nel file *web.config* dell'app stessa.</span><span class="sxs-lookup"><span data-stu-id="5e305-269">If IIS is configured at the system level to use dynamic compression, that setting can be disabled for an app with the **\<urlCompression>** element in the app's *web.config* file.</span></span> <span data-ttu-id="5e305-270">Per altre informazioni, vedere il [riferimento per la configurazione di \<system.webServer>](https://docs.microsoft.com/iis/configuration/system.webServer/), la [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) e [Uso dei moduli IIS con ASP.NET Core](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="5e305-270">For more information, see the [configuration reference for \<system.webServer>](https://docs.microsoft.com/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module) and [Using IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="5e305-271">Se è necessario impostare variabili di ambiente per singole app in esecuzione in pool di app isolate (supportati in IIS 10.0+), vedere la sezione *Comando AppCmd.exe* dell'argomento [Variabili di ambiente \<environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) nella documentazione di riferimento di IIS.</span><span class="sxs-lookup"><span data-stu-id="5e305-271">If there's a need to set environment variables for individual apps running in isolated app pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="5e305-272">Sezioni di configurazione di web.config</span><span class="sxs-lookup"><span data-stu-id="5e305-272">Configuration sections of web.config</span></span>

<span data-ttu-id="5e305-273">Le sezioni di configurazione di app ASP.NET Framework in *web.config* non vengono usate dalle app ASP.NET Core per la configurazione:</span><span class="sxs-lookup"><span data-stu-id="5e305-273">Configruation sections of ASP.NET Framework apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* <span data-ttu-id="5e305-274">**\<system.web>**</span><span class="sxs-lookup"><span data-stu-id="5e305-274">**\<system.web>**</span></span>
* <span data-ttu-id="5e305-275">**\<appSettings>**</span><span class="sxs-lookup"><span data-stu-id="5e305-275">**\<appSettings>**</span></span>
* <span data-ttu-id="5e305-276">**\<connectionStrings>**</span><span class="sxs-lookup"><span data-stu-id="5e305-276">**\<connectionStrings>**</span></span>
* <span data-ttu-id="5e305-277">**\<location>**</span><span class="sxs-lookup"><span data-stu-id="5e305-277">**\<location>**</span></span>

<span data-ttu-id="5e305-278">Le applicazioni ASP.NET Core vengono configurate tramite altri provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="5e305-278">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="5e305-279">Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="5e305-279">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="5e305-280">Pool di applicazioni</span><span class="sxs-lookup"><span data-stu-id="5e305-280">Application Pools</span></span>

<span data-ttu-id="5e305-281">Quando si ospitano più siti Web in un unico sistema, isolare le app le une dalle altre eseguendo ogni app nel pool di app corrispondente.</span><span class="sxs-lookup"><span data-stu-id="5e305-281">When hosting multiple websites on a single system, isolate the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="5e305-282">La finestra di dialogo **Aggiungi sito Web** di IIS ha questa configurazione come impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="5e305-282">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="5e305-283">Quando si specifica un valore in **Nome del sito**, il testo viene automaticamente trasferito alla casella di testo **Pool di applicazioni**.</span><span class="sxs-lookup"><span data-stu-id="5e305-283">When **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="5e305-284">Quando si aggiunge il sito Web viene creato un nuovo pool di app con il nome del sito.</span><span class="sxs-lookup"><span data-stu-id="5e305-284">A new app pool is created using the site name when the website is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="5e305-285">Identità del pool di applicazione</span><span class="sxs-lookup"><span data-stu-id="5e305-285">Application Pool Identity</span></span>

<span data-ttu-id="5e305-286">Un account di identità del pool di app consente di eseguire un'app in un account univoco, senza dover creare e gestire domini o account locali.</span><span class="sxs-lookup"><span data-stu-id="5e305-286">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="5e305-287">In IIS 8.0+ il processo di lavoro amministrazione IIS (WAS) crea un account virtuale con il nome del nuovo pool di app ed esegue i processi di lavoro del pool di applicazioni inclusi nell'account per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="5e305-287">On IIS 8.0+, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="5e305-288">Nella Console di gestione IIS in **Impostazioni avanzate** per il pool di app verificare che **Identità** sia impostata su **ApplicationPoolIdentity**:</span><span class="sxs-lookup"><span data-stu-id="5e305-288">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![Finestra di dialogo Impostazioni avanzate del pool di applicazione](index/_static/apppool-identity.png)

<span data-ttu-id="5e305-290">Il processo di gestione IIS crea un identificatore sicuro con il nome del pool di app nel sistema di sicurezza di Windows.</span><span class="sxs-lookup"><span data-stu-id="5e305-290">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="5e305-291">È possibile proteggere le risorse con questa identità, che tuttavia non è un account utente reale e non viene visualizzata nella Console di gestione utenti di Windows.</span><span class="sxs-lookup"><span data-stu-id="5e305-291">Resources can be secured by using this identity; however, this identity isn't a real user account and won't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="5e305-292">Se il processo di lavoro IIS richiede l'accesso con privilegi elevati all'app, modificare l'elenco di controllo di accesso (ACL) della directory contenente l'app:</span><span class="sxs-lookup"><span data-stu-id="5e305-292">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="5e305-293">Aprire Esplora risorse, quindi spostarsi nella directory.</span><span class="sxs-lookup"><span data-stu-id="5e305-293">Open Windows Explorer and navigate to the directory.</span></span>

2. <span data-ttu-id="5e305-294">Fare clic con il pulsante destro del mouse sulla directory e scegliere **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="5e305-294">Right-click on the directory and select **Properties**.</span></span>

3. <span data-ttu-id="5e305-295">Nella scheda **Sicurezza** selezionare il pulsante **Modifica** e quindi il pulsante **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="5e305-295">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

4. <span data-ttu-id="5e305-296">Fare clic sul pulsante **Percorsi** e verificare che il sistema sia selezionato.</span><span class="sxs-lookup"><span data-stu-id="5e305-296">Select the **Locations** button and make sure the system is selected.</span></span>

5. <span data-ttu-id="5e305-297">Immettere **IIS AppPool\DefaultAppPool** nella casella di testo **Immettere i nomi degli oggetti da selezionare**.</span><span class="sxs-lookup"><span data-stu-id="5e305-297">Enter **IIS AppPool\DefaultAppPool** in **Enter the object names to select** textbox.</span></span>

   ![Selezionare la finestra di dialogo degli utenti o dei gruppi per la cartella dell'app](index/_static/select-users-or-groups-1.png)

6. <span data-ttu-id="5e305-299">Fare clic sul pulsante **Controlla nomi**.</span><span class="sxs-lookup"><span data-stu-id="5e305-299">Select the **Check Names** button.</span></span> <span data-ttu-id="5e305-300">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="5e305-300">Select **OK**.</span></span>

   ![Selezionare la finestra di dialogo degli utenti o dei gruppi per la cartella dell'app](index/_static/select-users-or-groups-2.png)

<span data-ttu-id="5e305-302">Questa operazione può essere eseguita al prompt dei comandi usando lo strumento **ICACLS**:</span><span class="sxs-lookup"><span data-stu-id="5e305-302">This can also be accomplished via a command prompt using the **ICACLS** tool:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="additional-resources"></a><span data-ttu-id="5e305-303">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="5e305-303">Additional resources</span></span>

* [<span data-ttu-id="5e305-304">Risolvere i problemi di ASP.NET Core in IIS</span><span class="sxs-lookup"><span data-stu-id="5e305-304">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="5e305-305">Errori comuni di Azure App Service e IIS con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e305-305">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="5e305-306">Introduzione al modulo di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e305-306">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="5e305-307">Guida di riferimento per la configurazione del modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e305-307">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="5e305-308">Utilizzo dei moduli IIS con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e305-308">Using IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="5e305-309">Introduzione a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e305-309">Introduction to ASP.NET Core</span></span>](../index.md)
* [<span data-ttu-id="5e305-310">Il sito IIS ufficiale di Microsoft</span><span class="sxs-lookup"><span data-stu-id="5e305-310">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="5e305-311">Libreria di Microsoft TechNet: Windows Server</span><span class="sxs-lookup"><span data-stu-id="5e305-311">Microsoft TechNet Library: Windows Server</span></span>](https://docs.microsoft.com/windows-server/windows-server-versions)
