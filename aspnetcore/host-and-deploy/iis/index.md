---
title: Host ASP.NET Core in Windows con IIS
author: guardrex
description: Informazioni su come ospitare app ASP.NET Core in Windows Server Internet Information Services (IIS).
ms.author: riande
ms.custom: mvc
ms.date: 11/10/2018
uid: host-and-deploy/iis/index
ms.openlocfilehash: 1b34195dc51ca8dab5e8eda10f05ff6678fbc78c
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2018
ms.locfileid: "51570165"
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="1609e-103">Host ASP.NET Core in Windows con IIS</span><span class="sxs-lookup"><span data-stu-id="1609e-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="1609e-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1609e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[<span data-ttu-id="1609e-105">Installare il bundle di hosting .NET Core</span><span class="sxs-lookup"><span data-stu-id="1609e-105">Install the .NET Core Hosting Bundle</span></span>](#install-the-net-core-hosting-bundle)

## <a name="supported-operating-systems"></a><span data-ttu-id="1609e-106">Sistemi operativi supportati</span><span class="sxs-lookup"><span data-stu-id="1609e-106">Supported operating systems</span></span>

<span data-ttu-id="1609e-107">Sono supportati i sistemi operativi seguenti:</span><span class="sxs-lookup"><span data-stu-id="1609e-107">The following operating systems are supported:</span></span>

* <span data-ttu-id="1609e-108">Windows 7 o versione successiva</span><span class="sxs-lookup"><span data-stu-id="1609e-108">Windows 7 or later</span></span>
* <span data-ttu-id="1609e-109">Windows Server 2008 R2 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="1609e-109">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="1609e-110">Il [server HTTP.sys](xref:fundamentals/servers/httpsys) (chiamato in precedenza [WebListener](xref:fundamentals/servers/weblistener)) non funziona in una configurazione proxy inverso con IIS.</span><span class="sxs-lookup"><span data-stu-id="1609e-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) doesn't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="1609e-111">È necessario usare il [server Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="1609e-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="1609e-112">Per informazioni sull'hosting in Azure, vedere <xref:host-and-deploy/azure-apps/index>.</span><span class="sxs-lookup"><span data-stu-id="1609e-112">For information on hosting in Azure, see <xref:host-and-deploy/azure-apps/index>.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="1609e-113">Configurazione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="1609e-113">Application configuration</span></span>

### <a name="enable-the-iisintegration-components"></a><span data-ttu-id="1609e-114">Abilitare i componenti IISIntegration</span><span class="sxs-lookup"><span data-stu-id="1609e-114">Enable the IISIntegration components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="1609e-115">Un file *Program.cs* tipico chiama <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> per avviare la configurazione di un host:</span><span class="sxs-lookup"><span data-stu-id="1609e-115">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1609e-116">Un file *Program.cs* tipico chiama <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> per avviare la configurazione di un host:</span><span class="sxs-lookup"><span data-stu-id="1609e-116">A typical *Program.cs* calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to begin setting up a host:</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1609e-117">**Modello di hosting in-process**</span><span class="sxs-lookup"><span data-stu-id="1609e-117">**In-process hosting model**</span></span>

<span data-ttu-id="1609e-118">`CreateDefaultBuilder` chiama il metodo `UseIIS` per avviare [CoreCLR](/dotnet/standard/glossary#coreclr) ed eseguire l'hosting dell'app nel processo di lavoro IIS (*w3wp.exe* o *iisexpress.exe*).</span><span class="sxs-lookup"><span data-stu-id="1609e-118">`CreateDefaultBuilder` calls the `UseIIS` method to boot the [CoreCLR](/dotnet/standard/glossary#coreclr) and host the app inside of the IIS worker process (*w3wp.exe* or *iisexpress.exe*).</span></span> <span data-ttu-id="1609e-119">I test delle prestazioni indicano che l'hosting di un'app .NET Core in-process offre una velocità effettiva delle richieste significativamente superiore rispetto all'hosting dell'app out-of-process o all'inoltro delle richieste a [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="1609e-119">Performance tests indicate that hosting a .NET Core app in-process delivers significantly higher request throughput compared to hosting the app out-of-process and proxying requests to [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="1609e-120">**Modello di hosting out-of-process**</span><span class="sxs-lookup"><span data-stu-id="1609e-120">**Out-of-process hosting model**</span></span>

<span data-ttu-id="1609e-121">Per l'hosting out-of-process con IIS, `CreateDefaultBuilder` configura [Kestrel](xref:fundamentals/servers/kestrel) come server Web e abilita l'integrazione IIS configurando il percorso base e la porta per il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1609e-121">For out-of-process hosting with IIS, `CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="1609e-122">Il modulo ASP.NET Core genera una porta dinamica da assegnare al processo back-end.</span><span class="sxs-lookup"><span data-stu-id="1609e-122">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="1609e-123">`CreateDefaultBuilder` chiama il metodo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*>.</span><span class="sxs-lookup"><span data-stu-id="1609e-123">`CreateDefaultBuilder` calls the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> method.</span></span> <span data-ttu-id="1609e-124">`UseIISIntegration` configura Kestrel per l'ascolto sulla porta dinamica all'indirizzo IP localhost (`127.0.0.1`).</span><span class="sxs-lookup"><span data-stu-id="1609e-124">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="1609e-125">Se la porta dinamica è 1234, Kestrel è in ascolto su `127.0.0.1:1234`.</span><span class="sxs-lookup"><span data-stu-id="1609e-125">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="1609e-126">Questa configurazione sostituisce le altre configurazioni URL fornite da:</span><span class="sxs-lookup"><span data-stu-id="1609e-126">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="1609e-127">API Listen di Kestrel</span><span class="sxs-lookup"><span data-stu-id="1609e-127">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="1609e-128">[Configurazione](xref:fundamentals/configuration/index) (o [opzione URL della riga di comando](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="1609e-128">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="1609e-129">Le chiamate a `UseUrls` o all'API `Listen` di Kestrel non sono necessarie quando si usa il modulo.</span><span class="sxs-lookup"><span data-stu-id="1609e-129">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="1609e-130">In caso di chiamata di `UseUrls` o `Listen`, Kestrel resta in ascolto sulla porta specificata solo quando si esegue l'app senza IIS.</span><span class="sxs-lookup"><span data-stu-id="1609e-130">If `UseUrls` or `Listen` is called, Kestrel listens on the ports specified only when running the app without IIS.</span></span>

<span data-ttu-id="1609e-131">Per altre informazioni sui modelli di hosting in-process e out-of-process, vedere [Modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) e [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1609e-131">For more information on the in-process and out-of-process hosting models, see [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="1609e-132">`CreateDefaultBuilder` configura [Kestrel](xref:fundamentals/servers/kestrel) come server Web e abilita l'integrazione IIS configurando il percorso base e la porta per il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1609e-132">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="1609e-133">Il modulo ASP.NET Core genera una porta dinamica da assegnare al processo back-end.</span><span class="sxs-lookup"><span data-stu-id="1609e-133">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="1609e-134">`CreateDefaultBuilder` chiama il metodo [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration).</span><span class="sxs-lookup"><span data-stu-id="1609e-134">`CreateDefaultBuilder` calls the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) method.</span></span> <span data-ttu-id="1609e-135">`UseIISIntegration` configura Kestrel per l'ascolto sulla porta dinamica all'indirizzo IP localhost (`127.0.0.1`).</span><span class="sxs-lookup"><span data-stu-id="1609e-135">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`127.0.0.1`).</span></span> <span data-ttu-id="1609e-136">Se la porta dinamica è 1234, Kestrel è in ascolto su `127.0.0.1:1234`.</span><span class="sxs-lookup"><span data-stu-id="1609e-136">If the dynamic port is 1234, Kestrel listens at `127.0.0.1:1234`.</span></span> <span data-ttu-id="1609e-137">Questa configurazione sostituisce le altre configurazioni URL fornite da:</span><span class="sxs-lookup"><span data-stu-id="1609e-137">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="1609e-138">API Listen di Kestrel</span><span class="sxs-lookup"><span data-stu-id="1609e-138">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="1609e-139">[Configurazione](xref:fundamentals/configuration/index) (o [opzione URL della riga di comando](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="1609e-139">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="1609e-140">Le chiamate a `UseUrls` o all'API `Listen` di Kestrel non sono necessarie quando si usa il modulo.</span><span class="sxs-lookup"><span data-stu-id="1609e-140">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="1609e-141">In caso di chiamata di `UseUrls` o `Listen`, Kestrel resta in ascolto sulla porta specificata solo quando si esegue l'app senza IIS.</span><span class="sxs-lookup"><span data-stu-id="1609e-141">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1609e-142">`CreateDefaultBuilder` configura [Kestrel](xref:fundamentals/servers/kestrel) come server Web e abilita l'integrazione IIS configurando il percorso base e la porta per il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1609e-142">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>

<span data-ttu-id="1609e-143">Il modulo ASP.NET Core genera una porta dinamica da assegnare al processo back-end.</span><span class="sxs-lookup"><span data-stu-id="1609e-143">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="1609e-144">`CreateDefaultBuilder` chiama il metodo [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration).</span><span class="sxs-lookup"><span data-stu-id="1609e-144">`CreateDefaultBuilder` calls the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) method.</span></span> <span data-ttu-id="1609e-145">`UseIISIntegration` configura Kestrel per l'ascolto sulla porta dinamica all'indirizzo IP localhost (`localhost`).</span><span class="sxs-lookup"><span data-stu-id="1609e-145">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`localhost`).</span></span> <span data-ttu-id="1609e-146">Se la porta dinamica è 1234, Kestrel è in ascolto su `localhost:1234`.</span><span class="sxs-lookup"><span data-stu-id="1609e-146">If the dynamic port is 1234, Kestrel listens at `localhost:1234`.</span></span> <span data-ttu-id="1609e-147">Questa configurazione sostituisce le altre configurazioni URL fornite da:</span><span class="sxs-lookup"><span data-stu-id="1609e-147">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* [<span data-ttu-id="1609e-148">API Listen di Kestrel</span><span class="sxs-lookup"><span data-stu-id="1609e-148">Kestrel's Listen API</span></span>](xref:fundamentals/servers/kestrel#endpoint-configuration)
* <span data-ttu-id="1609e-149">[Configurazione](xref:fundamentals/configuration/index) (o [opzione URL della riga di comando](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="1609e-149">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="1609e-150">Le chiamate a `UseUrls` o all'API `Listen` di Kestrel non sono necessarie quando si usa il modulo.</span><span class="sxs-lookup"><span data-stu-id="1609e-150">Calls to `UseUrls` or Kestrel's `Listen` API aren't required when using the module.</span></span> <span data-ttu-id="1609e-151">In caso di chiamata di `UseUrls` o `Listen`, Kestrel resta in ascolto sulla porta specificata solo quando si esegue l'app senza IIS.</span><span class="sxs-lookup"><span data-stu-id="1609e-151">If `UseUrls` or `Listen` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1609e-152">Includere una dipendenza dal pacchetto [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) nelle dipendenze dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="1609e-152">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="1609e-153">Usare il middleware di integrazione IIS aggiungendo il metodo di estensione [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) a [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder):</span><span class="sxs-lookup"><span data-stu-id="1609e-153">Use IIS Integration middleware by adding the [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) extension method to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder):</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="1609e-154">[UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) e [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) sono entrambi obbligatori.</span><span class="sxs-lookup"><span data-stu-id="1609e-154">Both [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) and [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions.useiisintegration) are required.</span></span> <span data-ttu-id="1609e-155">Il fatto che il codice chiami `UseIISIntegration` non influisce sulla portabilità del codice stesso.</span><span class="sxs-lookup"><span data-stu-id="1609e-155">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="1609e-156">Se l'app non viene eseguita dietro IIS (ad esempio se viene eseguita direttamente in Kestrel), `UseIISIntegration` non esegue alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="1609e-156">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` doesn't operate.</span></span>

<span data-ttu-id="1609e-157">Il modulo ASP.NET Core genera una porta dinamica da assegnare al processo back-end.</span><span class="sxs-lookup"><span data-stu-id="1609e-157">The ASP.NET Core Module generates a dynamic port to assign to the backend process.</span></span> <span data-ttu-id="1609e-158">`UseIISIntegration` configura Kestrel per l'ascolto sulla porta dinamica all'indirizzo IP localhost (`localhost`).</span><span class="sxs-lookup"><span data-stu-id="1609e-158">`UseIISIntegration` configures Kestrel to listen on the dynamic port at the localhost IP address (`localhost`).</span></span> <span data-ttu-id="1609e-159">Se la porta dinamica è 1234, Kestrel è in ascolto su `localhost:1234`.</span><span class="sxs-lookup"><span data-stu-id="1609e-159">If the dynamic port is 1234, Kestrel listens at `localhost:1234`.</span></span> <span data-ttu-id="1609e-160">Questa configurazione sostituisce le altre configurazioni URL fornite da:</span><span class="sxs-lookup"><span data-stu-id="1609e-160">This configuration replaces other URL configurations provided by:</span></span>

* `UseUrls`
* <span data-ttu-id="1609e-161">[Configurazione](xref:fundamentals/configuration/index) (o [opzione URL della riga di comando](xref:fundamentals/host/web-host#override-configuration))</span><span class="sxs-lookup"><span data-stu-id="1609e-161">[Configuration](xref:fundamentals/configuration/index) (or [command-line --urls option](xref:fundamentals/host/web-host#override-configuration))</span></span>

<span data-ttu-id="1609e-162">Non è necessaria una chiamata a `UseUrls` quando si usa il modulo.</span><span class="sxs-lookup"><span data-stu-id="1609e-162">A call to `UseUrls` isn't required when using the module.</span></span> <span data-ttu-id="1609e-163">In caso di chiamata di `UseUrls`, Kestrel resta in ascolto sulla porta specificata solo quando si esegue l'app senza IIS.</span><span class="sxs-lookup"><span data-stu-id="1609e-163">If `UseUrls` is called, Kestrel listens on the port specified only when running the app without IIS.</span></span>

<span data-ttu-id="1609e-164">Se `UseUrls` viene chiamato in un'app ASP.NET Core 1.0, chiamarlo **prima** della chiamata di `UseIISIntegration` in modo che la porta configurata del modulo non venga sovrascritta.</span><span class="sxs-lookup"><span data-stu-id="1609e-164">If `UseUrls` is called in an ASP.NET Core 1.0 app, call it **before** calling `UseIISIntegration` so that the module-configured port isn't overwritten.</span></span> <span data-ttu-id="1609e-165">Questo ordine di chiamata non è obbligatorio con ASP.NET 1.1 Core perché l'impostazione del modulo ha la precedenza su `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="1609e-165">This calling order isn't required with ASP.NET Core 1.1 because the module setting overrides `UseUrls`.</span></span>

::: moniker-end

<span data-ttu-id="1609e-166">Per altre informazioni sull'hosting, vedere [Hosting in ASP.NET Core](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="1609e-166">For more information on hosting, see [Host in ASP.NET Core](xref:fundamentals/host/index).</span></span>

### <a name="iis-options"></a><span data-ttu-id="1609e-167">Opzioni IIS</span><span class="sxs-lookup"><span data-stu-id="1609e-167">IIS options</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1609e-168">**Modello di hosting in-process**</span><span class="sxs-lookup"><span data-stu-id="1609e-168">**In-process hosting model**</span></span>

<span data-ttu-id="1609e-169">Per configurare le opzioni di IIS Server, includere una configurazione del servizio per [IISServerOptions](/dotnet/api/microsoft.aspnetcore.builder.iisserveroptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span><span class="sxs-lookup"><span data-stu-id="1609e-169">To configure IIS Server options, include a service configuration for [IISServerOptions](/dotnet/api/microsoft.aspnetcore.builder.iisserveroptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span></span> <span data-ttu-id="1609e-170">L'esempio seguente disabilita AutomaticAuthentication:</span><span class="sxs-lookup"><span data-stu-id="1609e-170">The following example disables AutomaticAuthentication:</span></span>

```csharp
services.Configure<IISServerOptions>(options => 
{
    options.AutomaticAuthentication = false;
});
```

| <span data-ttu-id="1609e-171">Opzione</span><span class="sxs-lookup"><span data-stu-id="1609e-171">Option</span></span>                         | <span data-ttu-id="1609e-172">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="1609e-172">Default</span></span> | <span data-ttu-id="1609e-173">Impostazione</span><span class="sxs-lookup"><span data-stu-id="1609e-173">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="1609e-174">Se `true`, IIS Server imposta l'utente `HttpContext.User` autenticato tramite l'[autenticazione di Windows](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="1609e-174">If `true`, IIS Server sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="1609e-175">Se `false`, il server fornisce solo un'identità per `HttpContext.User` e risponde alle richieste esplicite di `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="1609e-175">If `false`, the server only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="1609e-176">Per il funzionamento di `AutomaticAuthentication` l’autenticazione di Windows deve essere abilitata in IIS.</span><span class="sxs-lookup"><span data-stu-id="1609e-176">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="1609e-177">Per altre informazioni, vedere [Autenticazione di Windows](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="1609e-177">For more information, see [Windows Authentication](xref:security/authentication/windowsauth).</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="1609e-178">Imposta il nome visualizzato dagli utenti nelle pagine di accesso.</span><span class="sxs-lookup"><span data-stu-id="1609e-178">Sets the display name shown to users on login pages.</span></span> |

<span data-ttu-id="1609e-179">**Modello di hosting out-of-process**</span><span class="sxs-lookup"><span data-stu-id="1609e-179">**Out-of-process hosting model**</span></span>

::: moniker-end

<span data-ttu-id="1609e-180">Per configurare le opzioni di IIS, includere una configurazione del servizio per [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span><span class="sxs-lookup"><span data-stu-id="1609e-180">To configure IIS options, include a service configuration for [IISOptions](/dotnet/api/microsoft.aspnetcore.builder.iisoptions) in [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices).</span></span> <span data-ttu-id="1609e-181">Nell'esempio seguente si impedisce all'app di popolare `HttpContext.Connection.ClientCertificate`:</span><span class="sxs-lookup"><span data-stu-id="1609e-181">The following example prevents the app from populating `HttpContext.Connection.ClientCertificate`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    options.ForwardClientCertificate = false;
});
```

| <span data-ttu-id="1609e-182">Opzione</span><span class="sxs-lookup"><span data-stu-id="1609e-182">Option</span></span>                         | <span data-ttu-id="1609e-183">Impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="1609e-183">Default</span></span> | <span data-ttu-id="1609e-184">Impostazione</span><span class="sxs-lookup"><span data-stu-id="1609e-184">Setting</span></span> |
| ------------------------------ | :-----: | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="1609e-185">Se `true`, il middleware di integrazione IIS imposta `HttpContext.User` autenticato tramite l'[autenticazione di Windows](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="1609e-185">If `true`, IIS Integration Middleware sets the `HttpContext.User` authenticated by [Windows Authentication](xref:security/authentication/windowsauth).</span></span> <span data-ttu-id="1609e-186">Se `false`, il middleware fornisce solo un'identità per `HttpContext.User` e risponde solo alle richieste esplicite di `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="1609e-186">If `false`, the middleware only provides an identity for `HttpContext.User` and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="1609e-187">Per il funzionamento di `AutomaticAuthentication` l’autenticazione di Windows deve essere abilitata in IIS.</span><span class="sxs-lookup"><span data-stu-id="1609e-187">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> <span data-ttu-id="1609e-188">Per altre informazioni, vedere l'argomento [Autenticazione di Windows](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="1609e-188">For more information, see the [Windows Authentication](xref:security/authentication/windowsauth) topic.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="1609e-189">Imposta il nome visualizzato dagli utenti nelle pagine di accesso.</span><span class="sxs-lookup"><span data-stu-id="1609e-189">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="1609e-190">Se è `true` ed è presente l’intestazione della richiesta `MS-ASPNETCORE-CLIENTCERT`, `HttpContext.Connection.ClientCertificate` viene popolato.</span><span class="sxs-lookup"><span data-stu-id="1609e-190">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="1609e-191">Scenari con server proxy e servizi di bilanciamento del carico</span><span class="sxs-lookup"><span data-stu-id="1609e-191">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="1609e-192">Il middleware di integrazione di IIS, che consente di configurare il middleware delle intestazioni inoltrate, e il modulo ASP.NET Core sono configurati per inoltrare lo schema (HTTP/HTTPS) e l'indirizzo IP remoto di origine della richiesta.</span><span class="sxs-lookup"><span data-stu-id="1609e-192">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="1609e-193">Potrebbero essere necessari interventi di configurazione aggiuntivi per le app ospitate dietro ulteriori server proxy e servizi di bilanciamento del carico.</span><span class="sxs-lookup"><span data-stu-id="1609e-193">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="1609e-194">Per altre informazioni, vedere [Configurare ASP.NET Core per l'utilizzo di server proxy e servizi di bilanciamento del carico](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="1609e-194">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="webconfig-file"></a><span data-ttu-id="1609e-195">File web.config</span><span class="sxs-lookup"><span data-stu-id="1609e-195">web.config file</span></span>

<span data-ttu-id="1609e-196">Il file *web.config* configura il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1609e-196">The *web.config* file configures the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="1609e-197">La creazione, la trasformazione e la pubblicazione del file *web.config* sono operazioni gestite da una destinazione MSBuild (`_TransformWebConfig`) quando viene pubblicato il progetto.</span><span class="sxs-lookup"><span data-stu-id="1609e-197">Creating, transforming, and publishing the *web.config* file is handled by an MSBuild target (`_TransformWebConfig`) when the project is published.</span></span> <span data-ttu-id="1609e-198">Questa destinazione si trova tra le destinazioni Web SDK (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="1609e-198">This target is present in the Web SDK targets (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="1609e-199">L'SDK è impostato all'inizio del file di progetto:</span><span class="sxs-lookup"><span data-stu-id="1609e-199">The SDK is set at the top of the project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
```

<span data-ttu-id="1609e-200">Se nel progetto non è presente un file *web.config*, il file viene creato con un valore *processPath* e *argomenti* corretti per la configurazione del [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). Il file viene quindi spostato nell'[output pubblicato](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="1609e-200">If a *web.config* file isn't present in the project, the file is created with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span>

<span data-ttu-id="1609e-201">Se nel progetto è presente un file *web.config*, il file viene trasformato con il valore di *processPath* e gli *argomenti* corretti per la configurazione del modulo ASP.NET Core, quindi viene spostato nell'output pubblicato.</span><span class="sxs-lookup"><span data-stu-id="1609e-201">If a *web.config* file is present in the project, the file is transformed with the correct *processPath* and *arguments* to configure the ASP.NET Core Module and moved to published output.</span></span> <span data-ttu-id="1609e-202">La trasformazione non modifica le impostazioni di configurazione di IIS incluse nel file.</span><span class="sxs-lookup"><span data-stu-id="1609e-202">The transformation doesn't modify IIS configuration settings in the file.</span></span>

<span data-ttu-id="1609e-203">Il file *web.config* può fornire ulteriori impostazioni di configurazione di IIS che controllano i moduli IIS attivi.</span><span class="sxs-lookup"><span data-stu-id="1609e-203">The *web.config* file may provide additional IIS configuration settings that control active IIS modules.</span></span> <span data-ttu-id="1609e-204">Per informazioni sui moduli IIS in grado di elaborare le richieste con le app di ASP.NET Core, vedere l'argomento [Moduli IIS](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="1609e-204">For information on IIS modules that are capable of processing requests with ASP.NET Core apps, see the [IIS modules](xref:host-and-deploy/iis/modules) topic.</span></span>

<span data-ttu-id="1609e-205">Per impedire che Web SDK trasformi il file *web.config*, usare la proprietà **\<IsTransformWebConfigDisabled >** nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="1609e-205">To prevent the Web SDK from transforming the *web.config* file, use the **\<IsTransformWebConfigDisabled>** property in the project file:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="1609e-206">Quando si disabilita la trasformazione del file in Web SDK, il valore di *processPath* e gli *argomenti* devono essere impostati manualmente dallo sviluppatore.</span><span class="sxs-lookup"><span data-stu-id="1609e-206">When disabling the Web SDK from transforming the file, the *processPath* and *arguments* should be manually set by the developer.</span></span> <span data-ttu-id="1609e-207">Per altre informazioni, vedere la [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1609e-207">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

### <a name="webconfig-file-location"></a><span data-ttu-id="1609e-208">Posizione del file web.config</span><span class="sxs-lookup"><span data-stu-id="1609e-208">web.config file location</span></span>

<span data-ttu-id="1609e-209">Per configurare correttamente il [modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module), il file *web.config* deve essere presente nel percorso radice del contenuto (in genere il percorso base dell'app) dell'app distribuita.</span><span class="sxs-lookup"><span data-stu-id="1609e-209">In order to set up the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) correctly, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app.</span></span> <span data-ttu-id="1609e-210">Corrisponde al percorso fisico del sito Web fornito a IIS.</span><span class="sxs-lookup"><span data-stu-id="1609e-210">This is the same location as the website physical path provided to IIS.</span></span> <span data-ttu-id="1609e-211">Il file *web.config* deve essere presente nella radice dell'app per abilitare la pubblicazione di più app mediante Distribuzione Web.</span><span class="sxs-lookup"><span data-stu-id="1609e-211">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="1609e-212">Nel percorso fisico dell'app sono presenti file riservati, come *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (commenti in formato documentazione XML) e *\<assembly>.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="1609e-212">Sensitive files exist on the app's physical path, such as *\<assembly>.runtimeconfig.json*, *\<assembly>.xml* (XML Documentation comments), and *\<assembly>.deps.json*.</span></span> <span data-ttu-id="1609e-213">Quando il file *web.config* è presente e il sito viene avviato normalmente, IIS non fornisce questi file riservati se vengono richiesti.</span><span class="sxs-lookup"><span data-stu-id="1609e-213">When the *web.config* file is present and and the site starts normally, IIS doesn't serve these sensitive files if they're requested.</span></span> <span data-ttu-id="1609e-214">Se il file *web.config* non è presente, ha un nome non corretto o non è in grado di configurare il sito per l'avvio normale, IIS potrebbe rendere disponibili pubblicamente i file riservati.</span><span class="sxs-lookup"><span data-stu-id="1609e-214">If the *web.config* file is missing, incorrectly named, or unable to configure the site for normal startup, IIS may serve sensitive files publicly.</span></span>

<span data-ttu-id="1609e-215">**Il file *web.config* deve essere sempre presente nella distribuzione, deve essere denominato correttamente ed essere in grado di configurare il sito per l'avvio normale. Non rimuovere mai il file *web.config* da una distribuzione in ambiente di produzione.**</span><span class="sxs-lookup"><span data-stu-id="1609e-215">**The *web.config* file must be present in the deployment at all times, correctly named, and able to configure the site for normal start up. Never remove the *web.config* file from a production deployment.**</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="1609e-216">Configurazione di IIS</span><span class="sxs-lookup"><span data-stu-id="1609e-216">IIS configuration</span></span>

<span data-ttu-id="1609e-217">**Sistemi operativi Windows Server**</span><span class="sxs-lookup"><span data-stu-id="1609e-217">**Windows Server operating systems**</span></span>

<span data-ttu-id="1609e-218">Abilitare il ruolo del server **Server Web (IIS)** e stabilire i servizi di ruolo.</span><span class="sxs-lookup"><span data-stu-id="1609e-218">Enable the **Web Server (IIS)** server role and establish role services.</span></span>

1. <span data-ttu-id="1609e-219">Usare la procedura guidata **Aggiungi ruoli e funzionalità** accessibile tramite il menu **Gestisci** o il collegamento in **Server Manager**.</span><span class="sxs-lookup"><span data-stu-id="1609e-219">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="1609e-220">Nel passaggio **Ruoli del server** selezionare la casella per **Server Web (IIS)**.</span><span class="sxs-lookup"><span data-stu-id="1609e-220">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

   ![Il ruolo Server Web IIS viene selezionato nel passaggio dei ruoli del server Selezionare.](index/_static/server-roles-ws2016.png)

1. <span data-ttu-id="1609e-222">Dopo il passaggio **Funzionalità**, viene caricato il passaggio**Servizi ruolo** per Server Web (IIS).</span><span class="sxs-lookup"><span data-stu-id="1609e-222">After the **Features** step, the **Role services** step loads for Web Server (IIS).</span></span> <span data-ttu-id="1609e-223">Selezionare i servizi ruolo IIS desiderati o accettare i servizi ruolo predefiniti specificati.</span><span class="sxs-lookup"><span data-stu-id="1609e-223">Select the IIS role services desired or accept the default role services provided.</span></span>

   ![I servizi ruolo predefiniti vengono selezionati nel passaggio Selezionare i servizi ruolo.](index/_static/role-services-ws2016.png)

   <span data-ttu-id="1609e-225">**Autenticazione di Windows (facoltativa)**</span><span class="sxs-lookup"><span data-stu-id="1609e-225">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="1609e-226">Per abilitare l'autenticazione di Windows, espandere i nodi seguenti: **Server Web** > **Sicurezza**.</span><span class="sxs-lookup"><span data-stu-id="1609e-226">To enable Windows Authentication, expand the following nodes: **Web Server** > **Security**.</span></span> <span data-ttu-id="1609e-227">Selezionare la funzionalità **Autenticazione di Windows**.</span><span class="sxs-lookup"><span data-stu-id="1609e-227">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="1609e-228">Per altre informazioni, vedere [Autenticazione di Windows\<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) e [Configurare l'autenticazione Windows](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="1609e-228">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="1609e-229">**WebSockets (facoltativo)**</span><span class="sxs-lookup"><span data-stu-id="1609e-229">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="1609e-230">WebSockets è supportato con ASP.NET Core 1.1 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="1609e-230">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="1609e-231">Per abilitare WebSockets, espandere i nodi seguenti: **Server Web** > **Sviluppo di applicazioni**.</span><span class="sxs-lookup"><span data-stu-id="1609e-231">To enable WebSockets, expand the following nodes: **Web Server** > **Application Development**.</span></span> <span data-ttu-id="1609e-232">Selezionare la funzionalità **Protocollo WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="1609e-232">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="1609e-233">Per altre informazioni, vedere [Oggetti WebSocket](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="1609e-233">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="1609e-234">Procedere con il passaggio **Conferma** per installare il ruolo del server web e i servizi.</span><span class="sxs-lookup"><span data-stu-id="1609e-234">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="1609e-235">Dopo l'installazione del ruolo **Server Web (IIS)** non è necessario riavviare il server o IIS.</span><span class="sxs-lookup"><span data-stu-id="1609e-235">A server/IIS restart isn't required after installing the **Web Server (IIS)** role.</span></span>

<span data-ttu-id="1609e-236">**Sistemi operativi desktop di Windows**</span><span class="sxs-lookup"><span data-stu-id="1609e-236">**Windows desktop operating systems**</span></span>

<span data-ttu-id="1609e-237">Abilitare **Console di gestione IIS** e **Servizi Web**.</span><span class="sxs-lookup"><span data-stu-id="1609e-237">Enable the **IIS Management Console** and **World Wide Web Services**.</span></span>

1. <span data-ttu-id="1609e-238">Passare a **Pannello di controllo** > **Programmi** > **Programmi e funzionalità** > **Attiva o disattiva funzionalità di Windows** (sul lato sinistro dello schermo).</span><span class="sxs-lookup"><span data-stu-id="1609e-238">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>

1. <span data-ttu-id="1609e-239">Aprire il nodo **Internet Information Services**.</span><span class="sxs-lookup"><span data-stu-id="1609e-239">Open the **Internet Information Services** node.</span></span> <span data-ttu-id="1609e-240">Aprire il nodo **Strumenti di gestione Web**.</span><span class="sxs-lookup"><span data-stu-id="1609e-240">Open the **Web Management Tools** node.</span></span>

1. <span data-ttu-id="1609e-241">Selezionare la casella per **Console di gestione IIS**.</span><span class="sxs-lookup"><span data-stu-id="1609e-241">Check the box for **IIS Management Console**.</span></span>

1. <span data-ttu-id="1609e-242">Selezionare la casella per **Servizi World Wide Web**.</span><span class="sxs-lookup"><span data-stu-id="1609e-242">Check the box for **World Wide Web Services**.</span></span>

1. <span data-ttu-id="1609e-243">Accettare le funzionalità predefinite per **Servizi World Wide Web** o personalizzare le funzionalità IIS.</span><span class="sxs-lookup"><span data-stu-id="1609e-243">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

   <span data-ttu-id="1609e-244">**Autenticazione di Windows (facoltativa)**</span><span class="sxs-lookup"><span data-stu-id="1609e-244">**Windows Authentication (Optional)**</span></span>  
   <span data-ttu-id="1609e-245">Per abilitare l'autenticazione di Windows, espandere i nodi seguenti: **Servizi Web** > **Sicurezza**.</span><span class="sxs-lookup"><span data-stu-id="1609e-245">To enable Windows Authentication, expand the following nodes: **World Wide Web Services** > **Security**.</span></span> <span data-ttu-id="1609e-246">Selezionare la funzionalità **Autenticazione di Windows**.</span><span class="sxs-lookup"><span data-stu-id="1609e-246">Select the **Windows Authentication** feature.</span></span> <span data-ttu-id="1609e-247">Per altre informazioni, vedere [Autenticazione di Windows\<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) e [Configurare l'autenticazione Windows](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="1609e-247">For more information, see [Windows Authentication \<windowsAuthentication>](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) and [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

   <span data-ttu-id="1609e-248">**WebSockets (facoltativo)**</span><span class="sxs-lookup"><span data-stu-id="1609e-248">**WebSockets (Optional)**</span></span>  
   <span data-ttu-id="1609e-249">WebSockets è supportato con ASP.NET Core 1.1 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="1609e-249">WebSockets is supported with ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="1609e-250">Per abilitare WebSockets, espandere i nodi seguenti: **Servizi Web** > **Funzionalità per lo sviluppo di applicazioni**.</span><span class="sxs-lookup"><span data-stu-id="1609e-250">To enable WebSockets, expand the following nodes: **World Wide Web Services** > **Application Development Features**.</span></span> <span data-ttu-id="1609e-251">Selezionare la funzionalità **Protocollo WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="1609e-251">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="1609e-252">Per altre informazioni, vedere [Oggetti WebSocket](xref:fundamentals/websockets).</span><span class="sxs-lookup"><span data-stu-id="1609e-252">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

1. <span data-ttu-id="1609e-253">Se l'installazione di IIS richiede un riavvio, riavviare il sistema.</span><span class="sxs-lookup"><span data-stu-id="1609e-253">If the IIS installation requires a restart, restart the system.</span></span>

![La console di gestione IIS e servizi World Wide Web sono selezionati nelle funzionalità di Windows.](index/_static/windows-features-win10.png)

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="1609e-255">Installare il bundle di hosting .NET Core</span><span class="sxs-lookup"><span data-stu-id="1609e-255">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="1609e-256">Installare il *bundle di hosting .NET Core* nel sistema di hosting.</span><span class="sxs-lookup"><span data-stu-id="1609e-256">Install the *.NET Core Hosting Bundle* on the hosting system.</span></span> <span data-ttu-id="1609e-257">L'aggregazione installa il runtime di .NET Core, la libreria di .NET Core e il [modulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1609e-257">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="1609e-258">Il modulo consente l'esecuzione delle app ASP.NET Core dietro IIS.</span><span class="sxs-lookup"><span data-stu-id="1609e-258">The module allows ASP.NET Core apps to run behind IIS.</span></span> <span data-ttu-id="1609e-259">Se il sistema non ha una connessione Internet, ottenere e installare [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) prima di installare il bundle di hosting .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1609e-259">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Hosting Bundle.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1609e-260">Se il bundle di hosting viene installato prima di IIS, è necessario riparare l'installazione del bundle.</span><span class="sxs-lookup"><span data-stu-id="1609e-260">If the Hosting Bundle is installed before IIS, the bundle installation must be repaired.</span></span> <span data-ttu-id="1609e-261">Eseguire di nuovo il programma di installazione del bundle di hosting dopo l'installazione di IIS.</span><span class="sxs-lookup"><span data-stu-id="1609e-261">Run the Hosting Bundle installer again after installing IIS.</span></span>

### <a name="direct-download-current-version"></a><span data-ttu-id="1609e-262">Download diretto (versione corrente)</span><span class="sxs-lookup"><span data-stu-id="1609e-262">Direct download (current version)</span></span>

<span data-ttu-id="1609e-263">Scaricare il programma di installazione mediante il collegamento seguente:</span><span class="sxs-lookup"><span data-stu-id="1609e-263">Download the installer using the following link:</span></span>

[<span data-ttu-id="1609e-264">Programma di installazione del bundle di hosting .NET Core corrente (download diretto)</span><span class="sxs-lookup"><span data-stu-id="1609e-264">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

### <a name="earlier-versions-of-the-installer"></a><span data-ttu-id="1609e-265">Versioni precedenti del programma di installazione</span><span class="sxs-lookup"><span data-stu-id="1609e-265">Earlier versions of the installer</span></span>

<span data-ttu-id="1609e-266">Per ottenere una versione precedente del programma di installazione:</span><span class="sxs-lookup"><span data-stu-id="1609e-266">To obtain an earlier version of the installer:</span></span>

1. <span data-ttu-id="1609e-267">Passare agli [archivi dei download per .NET](https://www.microsoft.com/net/download/archives).</span><span class="sxs-lookup"><span data-stu-id="1609e-267">Navigate to the [.NET download archives](https://www.microsoft.com/net/download/archives).</span></span>
1. <span data-ttu-id="1609e-268">In **.NET Core**, selezionare la versione di .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1609e-268">Under **.NET Core**, select the .NET Core version.</span></span>
1. <span data-ttu-id="1609e-269">Nella colonna **Run apps - Runtime** (App di esecuzione - Runtime), trovare la riga della versione di runtime di .NET Core desiderata.</span><span class="sxs-lookup"><span data-stu-id="1609e-269">In the **Run apps - Runtime** column, find the row of the .NET Core runtime version desired.</span></span>
1. <span data-ttu-id="1609e-270">Scaricare il programma di installazione tramite il collegamento **Runtime & Hosting Bundle** (Runtime e bundle di hosting).</span><span class="sxs-lookup"><span data-stu-id="1609e-270">Download the installer using the **Runtime & Hosting Bundle** link.</span></span>

> [!WARNING]
> <span data-ttu-id="1609e-271">Alcuni programmi di installazione contengono versioni che hanno terminato il loro ciclo di vita e non sono più supportate da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1609e-271">Some installers contain release versions that have reached their end of life (EOL) and are no longer supported by Microsoft.</span></span> <span data-ttu-id="1609e-272">Per altre informazioni, vedere i [criteri di supporto](https://www.microsoft.com/net/download/dotnet-core/2.0).</span><span class="sxs-lookup"><span data-stu-id="1609e-272">For more information, see the [support policy](https://www.microsoft.com/net/download/dotnet-core/2.0).</span></span>

### <a name="install-the-hosting-bundle"></a><span data-ttu-id="1609e-273">Installare il bundle di hosting</span><span class="sxs-lookup"><span data-stu-id="1609e-273">Install the Hosting Bundle</span></span>

1. <span data-ttu-id="1609e-274">Eseguire il programma di installazione nel server.</span><span class="sxs-lookup"><span data-stu-id="1609e-274">Run the installer on the server.</span></span> <span data-ttu-id="1609e-275">Quando si esegue il programma di installazione da un prompt dei comandi di amministratore sono disponibili le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="1609e-275">The following switches are available when running the installer from an administrator command prompt:</span></span>

   * <span data-ttu-id="1609e-276">`OPT_NO_ANCM=1` &ndash; Ignora l'installazione del modulo ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1609e-276">`OPT_NO_ANCM=1` &ndash; Skip installing the ASP.NET Core Module.</span></span>
   * <span data-ttu-id="1609e-277">`OPT_NO_RUNTIME=1` &ndash; Ignora l'installazione del runtime .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1609e-277">`OPT_NO_RUNTIME=1` &ndash; Skip installing the .NET Core runtime.</span></span>
   * <span data-ttu-id="1609e-278">`OPT_NO_SHAREDFX=1` &ndash; Ignora l'installazione del framework condiviso di ASP.NET (runtime ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="1609e-278">`OPT_NO_SHAREDFX=1` &ndash; Skip installing the ASP.NET Shared Framework (ASP.NET runtime).</span></span>
   * <span data-ttu-id="1609e-279">`OPT_NO_X86=1` &ndash; Ignora l'installazione dei runtime x86.</span><span class="sxs-lookup"><span data-stu-id="1609e-279">`OPT_NO_X86=1` &ndash; Skip installing x86 runtimes.</span></span> <span data-ttu-id="1609e-280">Usare questa opzione se si è certi che non verrà eseguito l'hosting di app a 32 bit.</span><span class="sxs-lookup"><span data-stu-id="1609e-280">Use this switch when you know that you won't be hosting 32-bit apps.</span></span> <span data-ttu-id="1609e-281">Se non esiste alcuna possibilità che in futuro venga eseguito l'hosting di app sia a 32 che a 64 bit, non usare questa opzione e installare entrambi i runtime.</span><span class="sxs-lookup"><span data-stu-id="1609e-281">If there's any chance that you will host both 32-bit and 64-bit apps in the future, don't use this switch and install both runtimes.</span></span>
1. <span data-ttu-id="1609e-282">Riavviare il sistema o eseguire **net stop was /y** seguito da **net start w3svc** da un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="1609e-282">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span> <span data-ttu-id="1609e-283">Il riavvio di IIS rileva una modifica alla variabile di ambiente di sistema PATH apportata dal programma di installazione.</span><span class="sxs-lookup"><span data-stu-id="1609e-283">Restarting IIS picks up a change to the system PATH, which is an environment variable, made by the installer.</span></span>

<span data-ttu-id="1609e-284">Se il programma di installazione del bundle di hosting Windows rileva che è necessario reimpostare IIS per poter completare l'installazione, procede con la reimpostazione.</span><span class="sxs-lookup"><span data-stu-id="1609e-284">If the Windows Hosting Bundle installer detects that IIS requires a reset in order to complete installation, the installer resets IIS.</span></span> <span data-ttu-id="1609e-285">Se il programma di installazione attiva una reimpostazione di IIS, tutti i siti Web e i pool di app IIS vengono riavviati.</span><span class="sxs-lookup"><span data-stu-id="1609e-285">If the installer triggers an IIS reset, all of the IIS app pools and websites are restarted.</span></span>

> [!NOTE]
> <span data-ttu-id="1609e-286">Per informazioni sulla configurazione condivisa di IIS, vedere [Modulo di ASP.NET Core con configurazione condivisa di IIS](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span><span class="sxs-lookup"><span data-stu-id="1609e-286">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="1609e-287">Installare la Distribuzione Web quando si pubblica con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1609e-287">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="1609e-288">Quando si distribuiscono app ai server con [Distribuzione Web](/iis/publish/using-web-deploy/introduction-to-web-deploy), installare la versione più recente di Distribuzione Web nel server.</span><span class="sxs-lookup"><span data-stu-id="1609e-288">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="1609e-289">Per installare Distribuzione Web usare l'[Installazione guidata piattaforma Web (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) o ottenere direttamente un programma di installazione in [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span><span class="sxs-lookup"><span data-stu-id="1609e-289">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="1609e-290">Il metodo preferito consiste nell'usare WebPI.</span><span class="sxs-lookup"><span data-stu-id="1609e-290">The preferred method is to use WebPI.</span></span> <span data-ttu-id="1609e-291">WebPI offre un'installazione autonoma e una configurazione per i provider di hosting.</span><span class="sxs-lookup"><span data-stu-id="1609e-291">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="1609e-292">Creare il sito IIS</span><span class="sxs-lookup"><span data-stu-id="1609e-292">Create the IIS site</span></span>

1. <span data-ttu-id="1609e-293">Nel sistema host creare una cartella per contenere le cartelle e i file pubblicati dell'app.</span><span class="sxs-lookup"><span data-stu-id="1609e-293">On the hosting system, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="1609e-294">Un layout di distribuzione di un'app è descritto nell'argomento [Struttura della directory](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="1609e-294">An app's deployment layout is described in the [Directory Structure](xref:host-and-deploy/directory-structure) topic.</span></span>

1. <span data-ttu-id="1609e-295">All'interno della nuova cartella creare una cartella *logs* per contenere i log stdout del modulo ASP.NET Core quando è abilitata la registrazione stdout.</span><span class="sxs-lookup"><span data-stu-id="1609e-295">Within the new folder, create a *logs* folder to hold ASP.NET Core Module stdout logs when stdout logging is enabled.</span></span> <span data-ttu-id="1609e-296">Se l'app viene distribuita con una cartella *logs* nel payload, ignorare questo passaggio.</span><span class="sxs-lookup"><span data-stu-id="1609e-296">If the app is deployed with a *logs* folder in the payload, skip this step.</span></span> <span data-ttu-id="1609e-297">Per istruzioni su come impostare MSBuild per la creazione automatica della cartella *logs* quando il progetto viene compilato in locale, vedere l'argomento [Struttura della directory](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="1609e-297">For instructions on how to enable MSBuild to create the *logs* folder automatically when the project is built locally, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="1609e-298">usare il log stdout solo per la risoluzione dei problemi di avvio delle app.</span><span class="sxs-lookup"><span data-stu-id="1609e-298">Only use the stdout log to troubleshoot app startup failures.</span></span> <span data-ttu-id="1609e-299">Non usarlo mai per la registrazione delle app di routine.</span><span class="sxs-lookup"><span data-stu-id="1609e-299">Never use stdout logging for routine app logging.</span></span> <span data-ttu-id="1609e-300">Non esiste alcun limite per le dimensioni dei file di log o il numero di file di log che è possibile creare.</span><span class="sxs-lookup"><span data-stu-id="1609e-300">There's no limit on log file size or the number of log files created.</span></span> <span data-ttu-id="1609e-301">Il pool di applicazioni deve avere accesso in scrittura alla posizione in cui vengono scritti i log.</span><span class="sxs-lookup"><span data-stu-id="1609e-301">The app pool must have write access to the location where the logs are written.</span></span> <span data-ttu-id="1609e-302">Tutte le cartelle nel percorso della posizione del log devono essere già esistenti.</span><span class="sxs-lookup"><span data-stu-id="1609e-302">All of the folders on the path to the log location must exist.</span></span> <span data-ttu-id="1609e-303">Per altre informazioni sul log stdout, vedere [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) (Creazione e reindirizzamento dei log).</span><span class="sxs-lookup"><span data-stu-id="1609e-303">For more information on the stdout log, see [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span> <span data-ttu-id="1609e-304">Per informazioni sulla registrazione in un'app ASP.NET Core, vedere l'argomento [Registrazione](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="1609e-304">For information on logging in an ASP.NET Core app, see the [Logging](xref:fundamentals/logging/index) topic.</span></span>

1. <span data-ttu-id="1609e-305">In **Gestione IIS** aprire il nodo del server nel pannello **Connessioni**.</span><span class="sxs-lookup"><span data-stu-id="1609e-305">In **IIS Manager**, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="1609e-306">Fare clic con il pulsante destro del mouse sulla cartella **Siti**.</span><span class="sxs-lookup"><span data-stu-id="1609e-306">Right-click the **Sites** folder.</span></span> <span data-ttu-id="1609e-307">Scegliere **Aggiungi sito Web** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="1609e-307">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="1609e-308">Specificare un **Nome del sito** e impostare il **Percorso fisico** per la cartella di distribuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="1609e-308">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="1609e-309">Specificare la configurazione in **Binding** e creare il sito Web scegliendo **OK**:</span><span class="sxs-lookup"><span data-stu-id="1609e-309">Provide the **Binding** configuration and create the website by selecting **OK**:</span></span>

   ![Specificare il nome del sito, il percorso fisico e il nome Host nel passaggio Aggiungi sito Web.](index/_static/add-website-ws2016.png)

   > [!WARNING]
   > <span data-ttu-id="1609e-311">Le associazioni con caratteri jolly di livello superiore (`http://*:80/` e `http://+:80`) **non** devono essere usate,</span><span class="sxs-lookup"><span data-stu-id="1609e-311">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="1609e-312">poiché possono introdurre vulnerabilità a livello di sicurezza nell'app.</span><span class="sxs-lookup"><span data-stu-id="1609e-312">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="1609e-313">Questo concetto vale sia per i caratteri jolly sicuri che vulnerabili.</span><span class="sxs-lookup"><span data-stu-id="1609e-313">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="1609e-314">Usare nomi host espliciti al posto di caratteri jolly.</span><span class="sxs-lookup"><span data-stu-id="1609e-314">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="1609e-315">L'associazione con caratteri jolly del sottodominio (ad esempio, `*.mysub.com`) non costituisce un rischio per la sicurezza se viene controllato l'intero dominio padre (a differenza di `*.com`, che è vulnerabile).</span><span class="sxs-lookup"><span data-stu-id="1609e-315">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="1609e-316">Vedere la [sezione 5.4 di RFC7230](https://tools.ietf.org/html/rfc7230#section-5.4) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="1609e-316">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

1. <span data-ttu-id="1609e-317">Nel nodo del server selezionare **Pool di applicazioni**.</span><span class="sxs-lookup"><span data-stu-id="1609e-317">Under the server's node, select **Application Pools**.</span></span>

1. <span data-ttu-id="1609e-318">Fare clic con il pulsante destro del mouse sul pool di applicazioni del sito e scegliere **Impostazioni di base** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="1609e-318">Right-click the site's app pool and select **Basic Settings** from the contextual menu.</span></span>

1. <span data-ttu-id="1609e-319">Nella finestra **Modifica pool di applicazioni** impostare **Versione .NET CLR** su **Nessun codice gestito**.</span><span class="sxs-lookup"><span data-stu-id="1609e-319">In the **Edit Application Pool** window, set the **.NET CLR version** to **No Managed Code**:</span></span>

   ![Impostare Nessun codice gestito per la versione .NET CLR.](index/_static/edit-apppool-ws2016.png)

    <span data-ttu-id="1609e-321">ASP.NET Core viene eseguito in un processo separato e gestisce il runtime.</span><span class="sxs-lookup"><span data-stu-id="1609e-321">ASP.NET Core runs in a separate process and manages the runtime.</span></span> <span data-ttu-id="1609e-322">ASP.NET Core non si basa sul caricamento del CLR del desktop.</span><span class="sxs-lookup"><span data-stu-id="1609e-322">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span> <span data-ttu-id="1609e-323">L'impostazione della **versione .NET CLR** su **Nessun codice gestito** è facoltativa.</span><span class="sxs-lookup"><span data-stu-id="1609e-323">Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span>

1. <span data-ttu-id="1609e-324">Confermare che l'identità del modello del processo disponga delle autorizzazioni appropriate.</span><span class="sxs-lookup"><span data-stu-id="1609e-324">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="1609e-325">Se l'identità predefinita del pool di app (**Modello di processo** > **Identità**) viene modificata da **ApplicationPoolIdentity** a un'altra identità, verificare che la nuova identità disponga delle autorizzazioni necessarie per l'accesso alla cartella dell'app, al database e alle altre risorse richieste.</span><span class="sxs-lookup"><span data-stu-id="1609e-325">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span> <span data-ttu-id="1609e-326">Ad esempio, il pool di applicazioni richiede l'accesso in lettura e scrittura alle cartelle in cui l'app legge e scrive i file.</span><span class="sxs-lookup"><span data-stu-id="1609e-326">For example, the app pool requires read and write access to folders where the app reads and writes files.</span></span>

<span data-ttu-id="1609e-327">**Configurazione dell'autenticazione di Windows (facoltativa)**</span><span class="sxs-lookup"><span data-stu-id="1609e-327">**Windows Authentication configuration (Optional)**</span></span>  
<span data-ttu-id="1609e-328">Per altre informazioni, vedere [Configurare l'autenticazione Windows](xref:security/authentication/windowsauth).</span><span class="sxs-lookup"><span data-stu-id="1609e-328">For more information, see [Configure Windows authentication](xref:security/authentication/windowsauth).</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="1609e-329">Distribuire l'app</span><span class="sxs-lookup"><span data-stu-id="1609e-329">Deploy the app</span></span>

<span data-ttu-id="1609e-330">Distribuire l'app nella cartella creata nel sistema host.</span><span class="sxs-lookup"><span data-stu-id="1609e-330">Deploy the app to the folder created on the hosting system.</span></span> <span data-ttu-id="1609e-331">Il metodo consigliato per la distribuzione è [Distribuzione Web](/iis/publish/using-web-deploy/introduction-to-web-deploy).</span><span class="sxs-lookup"><span data-stu-id="1609e-331">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="1609e-332">Distribuzione web con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1609e-332">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="1609e-333">Vedere l'argomento [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) (Profili di pubblicazione Visual Studio per la distribuzione di app ASP.NET Core) per informazioni su come creare un profilo di pubblicazione da usare con Distribuzione Web.</span><span class="sxs-lookup"><span data-stu-id="1609e-333">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="1609e-334">Se il provider di hosting fornisce un profilo di pubblicazione o il supporto per crearne uno, scaricare il profilo e importarlo usando la finestra di dialogo **Pubblica** di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1609e-334">If the hosting provider provides a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![Pagina della finestra di dialogo Pubblica](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="1609e-336">Distribuzione Web all'esterno di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1609e-336">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="1609e-337">È anche possibile usare [Distribuzione Web](/iis/publish/using-web-deploy/introduction-to-web-deploy) all'esterno di Visual Studio dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="1609e-337">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="1609e-338">Per altre informazioni sulla distribuzione, vedere [Strumento Distribuzione Web](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span><span class="sxs-lookup"><span data-stu-id="1609e-338">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="1609e-339">Alternative a Distribuzione web</span><span class="sxs-lookup"><span data-stu-id="1609e-339">Alternatives to Web Deploy</span></span>

<span data-ttu-id="1609e-340">Usare uno dei metodi disponibili, ad esempio la copia manuale, Xcopy, Robocopy o PowerShell, per spostare l'app nel sistema host.</span><span class="sxs-lookup"><span data-stu-id="1609e-340">Use any of several methods to move the app to the hosting system, such as manual copy, Xcopy, Robocopy, or PowerShell.</span></span>

<span data-ttu-id="1609e-341">Per altre informazioni sulla distribuzione di ASP.NET Core in IIS, vedere la sezione [Risorse di distribuzione per amministratori IIS](#deployment-resources-for-iis-administrators).</span><span class="sxs-lookup"><span data-stu-id="1609e-341">For more information on ASP.NET Core deployment to IIS, see the [Deployment resources for IIS administrators](#deployment-resources-for-iis-administrators) section.</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="1609e-342">Esplorare il sito Web</span><span class="sxs-lookup"><span data-stu-id="1609e-342">Browse the website</span></span>

![Il browser Microsoft Edge ha caricato la pagina di avvio IIS.](index/_static/browsewebsite.png)

## <a name="locked-deployment-files"></a><span data-ttu-id="1609e-344">File di distribuzione bloccati</span><span class="sxs-lookup"><span data-stu-id="1609e-344">Locked deployment files</span></span>

<span data-ttu-id="1609e-345">I file nella cartella di distribuzione sono bloccati durante l'esecuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="1609e-345">Files in the deployment folder are locked when the app is running.</span></span> <span data-ttu-id="1609e-346">I file bloccati non possono essere sovrascritti durante la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="1609e-346">Locked files can't be overwritten during deployment.</span></span> <span data-ttu-id="1609e-347">Per rilasciare i file bloccati in una distribuzione, arrestare il pool di applicazioni usando **uno** dei metodi seguenti:</span><span class="sxs-lookup"><span data-stu-id="1609e-347">To release locked files in a deployment, stop the app pool using **one** of the following approaches:</span></span>

* <span data-ttu-id="1609e-348">Usare Distribuzione Web e fare riferimento a `Microsoft.NET.Sdk.Web` nel file di progetto.</span><span class="sxs-lookup"><span data-stu-id="1609e-348">Use Web Deploy and reference `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="1609e-349">Nella radice della directory dell'app web viene posizionato un file *app_offline.htm*.</span><span class="sxs-lookup"><span data-stu-id="1609e-349">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="1609e-350">Quando il file è presente, il modulo ASP.NET Core arresta normalmente l'app e rende disponibile il file *app_offline.htm* durante la distribuzione.</span><span class="sxs-lookup"><span data-stu-id="1609e-350">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="1609e-351">Per altre informazioni, vedere la [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="1609e-351">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>
* <span data-ttu-id="1609e-352">Arrestare manualmente il pool di applicazioni in Gestione IIS sul server.</span><span class="sxs-lookup"><span data-stu-id="1609e-352">Manually stop the app pool in the IIS Manager on the server.</span></span>
* <span data-ttu-id="1609e-353">Usare PowerShell per eliminare *app_offline.html* (è richiesto PowerShell 5 o versione successiva):</span><span class="sxs-lookup"><span data-stu-id="1609e-353">Use PowerShell to drop *app_offline.html* (requires PowerShell 5 or later):</span></span>

  ```PowerShell
  $pathToApp = 'PATH_TO_APP'

  # Stop the AppPool
  New-Item -Path $pathToApp app_offline.htm

  # Provide script commands here to deploy the app

  # Restart the AppPool
  Remove-Item -Path $pathToApp app_offline.htm

  ```

## <a name="data-protection"></a><span data-ttu-id="1609e-354">Protezione dei dati</span><span class="sxs-lookup"><span data-stu-id="1609e-354">Data protection</span></span>

<span data-ttu-id="1609e-355">Lo [stack di protezione dei dati di ASP.NET Core](xref:security/data-protection/introduction) viene usato da diversi [middleware](xref:fundamentals/middleware/index) di ASP.NET Core, inclusi quelli usati nell'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="1609e-355">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including middleware used in authentication.</span></span> <span data-ttu-id="1609e-356">Anche se le DPAPI (Data Protection API) non vengono chiamate dal codice dell'utente, è consigliabile configurare la protezione dati per la creazione di un [archivio di chiavi](xref:security/data-protection/implementation/key-management) crittografiche permanente con uno script di distribuzione o nel codice dell'utente.</span><span class="sxs-lookup"><span data-stu-id="1609e-356">Even if Data Protection APIs aren't called by user code, data protection should be configured with a deployment script or in user code to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="1609e-357">Se non si configura la protezione dei dati, le chiavi vengono mantenute in memoria ed eliminate al riavvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="1609e-357">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="1609e-358">Se il gruppo di chiavi viene archiviato in memoria quando l'app viene riavviata:</span><span class="sxs-lookup"><span data-stu-id="1609e-358">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="1609e-359">Tutti i token di autenticazione basati su cookie vengono invalidati.</span><span class="sxs-lookup"><span data-stu-id="1609e-359">All cookie-based authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="1609e-360">Gli utenti devono ripetere l'accesso alla richiesta successiva.</span><span class="sxs-lookup"><span data-stu-id="1609e-360">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="1609e-361">Tutti i dati protetti con il gruppo di chiavi non possono più essere decrittografati.</span><span class="sxs-lookup"><span data-stu-id="1609e-361">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="1609e-362">Possono essere inclusi i [token CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) e i [cookie TempData di ASP.NET Core MVC](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="1609e-362">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="1609e-363">Per configurare la protezione dati in IIS in modo da rendere permanente il gruppo di chiavi, usare **uno** degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="1609e-363">To configure data protection under IIS to persist the key ring, use **one** of the following approaches:</span></span>

* <span data-ttu-id="1609e-364">**Creare chiavi del Registro di sistema di protezione dati**</span><span class="sxs-lookup"><span data-stu-id="1609e-364">**Create Data Protection Registry Keys**</span></span>

  <span data-ttu-id="1609e-365">Le chiavi di protezione dati usate dalle app ASP.NET Core vengono archiviate nel Registro di sistema esterno alle app.</span><span class="sxs-lookup"><span data-stu-id="1609e-365">Data protection keys used by ASP.NET Core apps are stored in the registry external to the apps.</span></span> <span data-ttu-id="1609e-366">Per mantenere le chiavi per una determinata app, creare chiavi del Registro di sistema per il pool di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="1609e-366">To persist the keys for a given app, create registry keys for the app pool.</span></span>

  <span data-ttu-id="1609e-367">Per le installazioni IIS autonome non web farm è possibile usare lo [script PowerShell Data Protection Provision-AutoGenKeys.ps1 (Provisioning di protezione dati-AutoGenKeys.ps1)](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) per ogni pool di app usato con un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1609e-367">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/AspNetCore/blob/master/src/DataProtection/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="1609e-368">Questo script crea una chiave del Registro di sistema nel registro HKLM accessibile solo all'account del processo di lavoro del pool di applicazioni dell'app.</span><span class="sxs-lookup"><span data-stu-id="1609e-368">This script creates a registry key in the HKLM registry that's accessible only to the worker process account of the app's app pool.</span></span> <span data-ttu-id="1609e-369">Le chiavi vengono crittografate a riposo tramite DPAPI con una chiave a livello del computer.</span><span class="sxs-lookup"><span data-stu-id="1609e-369">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

  <span data-ttu-id="1609e-370">In scenari di web farm un'app può essere configurata in modo da usare un percorso UNC in cui archiviare il proprio gruppo di chiavi di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="1609e-370">In web farm scenarios, an app can be configured to use a UNC path to store its data protection key ring.</span></span> <span data-ttu-id="1609e-371">Per impostazione predefinita, le chiavi di protezione dati non vengono crittografate.</span><span class="sxs-lookup"><span data-stu-id="1609e-371">By default, the data protection keys aren't encrypted.</span></span> <span data-ttu-id="1609e-372">Assicurarsi che le autorizzazioni file per la condivisione di rete siano limitate all'account di Windows in cui viene eseguita l'app.</span><span class="sxs-lookup"><span data-stu-id="1609e-372">Ensure that the file permissions for the network share are limited to the Windows account the app runs under.</span></span> <span data-ttu-id="1609e-373">È possibile usare un certificato X509 per la protezione delle chiavi inattive.</span><span class="sxs-lookup"><span data-stu-id="1609e-373">An X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="1609e-374">Considerare l'implementazione di un meccanismo per consentire agli utenti di caricare i certificati: inserire i certificati nell'archivio certificati attendibili dell'utente e assicurarsi che siano disponibili in tutti i computer in cui viene eseguita l'app dell'utente.</span><span class="sxs-lookup"><span data-stu-id="1609e-374">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="1609e-375">Vedere [Configurare la protezione dati di ASP.NET Core](xref:security/data-protection/configuration/overview) per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="1609e-375">See [Configure ASP.NET Core Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

* <span data-ttu-id="1609e-376">**Configurare il pool di applicazioni IIS per caricare il profilo utente**</span><span class="sxs-lookup"><span data-stu-id="1609e-376">**Configure the IIS Application Pool to load the user profile**</span></span>

  <span data-ttu-id="1609e-377">Questa impostazione si trova nella sezione **Modello del processo** in **Impostazioni avanzate** per il pool di app.</span><span class="sxs-lookup"><span data-stu-id="1609e-377">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="1609e-378">Impostare Carica profilo utente su `True`.</span><span class="sxs-lookup"><span data-stu-id="1609e-378">Set Load User Profile to `True`.</span></span> <span data-ttu-id="1609e-379">Questa operazione archivia le chiavi nella directory del profilo utente e le mantiene protette tramite DPAPI con una chiave specifica dell'account utente usato dal pool di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="1609e-379">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used by the app pool.</span></span>

* <span data-ttu-id="1609e-380">**Usare il file system come archivio del gruppo di chiavi**</span><span class="sxs-lookup"><span data-stu-id="1609e-380">**Use the file system as a key ring store**</span></span>

  <span data-ttu-id="1609e-381">Modificare il codice dell'app per [usare il file system come archivio del gruppo di chiavi](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="1609e-381">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="1609e-382">Usare un certificato X509 per proteggere il gruppo di chiavi e verificare che sia un certificato attendibile.</span><span class="sxs-lookup"><span data-stu-id="1609e-382">Use an X509 certificate to protect the key ring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="1609e-383">Se il certificato è autofirmato, inserirlo nell'archivio radice attendibile.</span><span class="sxs-lookup"><span data-stu-id="1609e-383">If the certificate is self-signed, place the certificate in the Trusted Root store.</span></span>

  <span data-ttu-id="1609e-384">Quando si usa IIS in una web farm:</span><span class="sxs-lookup"><span data-stu-id="1609e-384">When using IIS in a web farm:</span></span>

  * <span data-ttu-id="1609e-385">Usare una condivisione di file a cui possono accedere tutti i computer.</span><span class="sxs-lookup"><span data-stu-id="1609e-385">Use a file share that all machines can access.</span></span>
  * <span data-ttu-id="1609e-386">Distribuire un certificato X509 in ogni computer.</span><span class="sxs-lookup"><span data-stu-id="1609e-386">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="1609e-387">Configurare [protezione dei dati nel codice](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="1609e-387">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

* <span data-ttu-id="1609e-388">**Impostare criteri a livello di computer per la protezione dei dati**</span><span class="sxs-lookup"><span data-stu-id="1609e-388">**Set a machine-wide policy for data protection**</span></span>

  <span data-ttu-id="1609e-389">Il sistema di protezione dei dati ha un supporto limitato per l'impostazione di [criteri a livello di computer](xref:security/data-protection/configuration/machine-wide-policy) predefiniti per tutte le app che usano le API di protezione dei dati.</span><span class="sxs-lookup"><span data-stu-id="1609e-389">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="1609e-390">Per ulteriori informazioni, vedere <xref:security/data-protection/introduction>.</span><span class="sxs-lookup"><span data-stu-id="1609e-390">For more information, see <xref:security/data-protection/introduction>.</span></span>

## <a name="sub-application-configuration"></a><span data-ttu-id="1609e-391">Configurazione delle applicazioni secondarie</span><span class="sxs-lookup"><span data-stu-id="1609e-391">Sub-application configuration</span></span>

<span data-ttu-id="1609e-392">Le sotto-app aggiunte sotto l'app radice non devono includere il modulo ASP.NET Core come gestore.</span><span class="sxs-lookup"><span data-stu-id="1609e-392">Sub-apps added under the root app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="1609e-393">Se il modulo viene aggiunto come gestore nel file *web.config* di un'app secondaria, quando si prova a esplorare l'app secondaria si riceve un messaggio 500.19 *(errore interno del server)* che segnala errori nel file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="1609e-393">If the module is added as a handler in a sub-app's *web.config* file, a *500.19 Internal Server Error* referencing the faulty config file is received when attempting to browse the sub-app.</span></span>

<span data-ttu-id="1609e-394">L'esempio seguente illustra un file *web.config* pubblicato per un'app secondaria di ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="1609e-394">The following example shows a published *web.config* file for an ASP.NET Core sub-app:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <aspNetCore processPath="dotnet" 
        arguments=".\MyApp.dll" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
    </system.webServer>
  </location>
</configuration>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="1609e-395">Se si ospita una sotto-app non ASP.NET Core in un'app ASP.NET Core, è necessario rimuovere in modo esplicito il gestore ereditato nel file *web.config* della sotto-app:</span><span class="sxs-lookup"><span data-stu-id="1609e-395">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore" />
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\MyApp.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

::: moniker-end

<span data-ttu-id="1609e-396">Per altre informazioni sulla configurazione del modulo di ASP.NET Core, vedere l'argomento [Introduzione al modulo di ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) e la [Guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="1609e-396">For more information on configuring the ASP.NET Core Module, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="1609e-397">Configurazione di IIS con web.config</span><span class="sxs-lookup"><span data-stu-id="1609e-397">Configuration of IIS with web.config</span></span>

<span data-ttu-id="1609e-398">La configurazione di IIS è influenzata dalla sezione **\<system.webServer>** di *web.config* per le funzionalità IIS valide per una configurazione proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="1609e-398">IIS configuration is influenced by the **\<system.webServer>** section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="1609e-399">Se IIS è configurato a livello di server per l'uso della compressione dinamica, l'elemento **\<urlCompression>** nel file *web.config* dell'app può disabilitare questa impostazione.</span><span class="sxs-lookup"><span data-stu-id="1609e-399">If IIS is configured at the server level to use dynamic compression, the **\<urlCompression>** element in the app's *web.config* file can disable it.</span></span>

<span data-ttu-id="1609e-400">Per altre informazioni, vedere la [guida di riferimento per la configurazione di \<system.webServer>](/iis/configuration/system.webServer/), la [guida di riferimento per la configurazione del modulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) e [Moduli IIS con ASP.NET Core](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="1609e-400">For more information, see the [configuration reference for \<system.webServer>](/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module), and [IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="1609e-401">Per impostare variabili di ambiente per singole app in esecuzione in pool di applicazioni isolate (supportati per IIS 10.0 o versioni successive), vedere la sezione *Comando AppCmd.exe* dell'argomento [Variabili di ambiente \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) nella documentazione di riferimento di IIS.</span><span class="sxs-lookup"><span data-stu-id="1609e-401">To set environment variables for individual apps running in isolated app pools (supported for IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="1609e-402">Sezioni di configurazione di web.config</span><span class="sxs-lookup"><span data-stu-id="1609e-402">Configuration sections of web.config</span></span>

<span data-ttu-id="1609e-403">Le sezioni di configurazione di app ASP.NET 4.x in *web.config* non vengono usate dalle app ASP.NET Core per la configurazione:</span><span class="sxs-lookup"><span data-stu-id="1609e-403">Configuration sections of ASP.NET 4.x apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* <span data-ttu-id="1609e-404">**\<system.web>**</span><span class="sxs-lookup"><span data-stu-id="1609e-404">**\<system.web>**</span></span>
* <span data-ttu-id="1609e-405">**\<appSettings>**</span><span class="sxs-lookup"><span data-stu-id="1609e-405">**\<appSettings>**</span></span>
* <span data-ttu-id="1609e-406">**\<connectionStrings>**</span><span class="sxs-lookup"><span data-stu-id="1609e-406">**\<connectionStrings>**</span></span>
* <span data-ttu-id="1609e-407">**\<location>**</span><span class="sxs-lookup"><span data-stu-id="1609e-407">**\<location>**</span></span>

<span data-ttu-id="1609e-408">Le applicazioni ASP.NET Core vengono configurate tramite altri provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="1609e-408">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="1609e-409">Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="1609e-409">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="1609e-410">Pool di applicazioni</span><span class="sxs-lookup"><span data-stu-id="1609e-410">Application Pools</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1609e-411">L'isolamento dei pool di app è determinato dal modello di hosting:</span><span class="sxs-lookup"><span data-stu-id="1609e-411">App pool isolation is determined by the hosting model:</span></span>

* <span data-ttu-id="1609e-412">Hosting in-process &ndash; Le app devono essere eseguite in pool di app separati.</span><span class="sxs-lookup"><span data-stu-id="1609e-412">In-process hosting &ndash; Apps are required to run in separate app pools.</span></span>
* <span data-ttu-id="1609e-413">Hosting out-of-process &ndash; È consigliabile isolare le app le une dalle altre eseguendo ogni app nel relativo pool di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="1609e-413">Out-of-process hosting &ndash; We recommend isolating the apps from each other by running each app in its own app pool.</span></span>

<span data-ttu-id="1609e-414">Nella finestra di dialogo **Aggiungi sito Web** è selezionato per impostazione predefinita un singolo pool di app per ogni app.</span><span class="sxs-lookup"><span data-stu-id="1609e-414">The IIS **Add Website** dialog defaults to a single app pool per app.</span></span> <span data-ttu-id="1609e-415">Quando si specifica un valore in **Nome del sito**, il testo viene automaticamente trasferito alla casella di testo **Pool di applicazioni**.</span><span class="sxs-lookup"><span data-stu-id="1609e-415">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="1609e-416">Quando si aggiunge il sito viene creato un nuovo pool di applicazioni con il nome del sito.</span><span class="sxs-lookup"><span data-stu-id="1609e-416">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="1609e-417">Quando si ospitano più siti Web in un server, è consigliabile isolare le app le une dalle altre eseguendo ogni app nel relativo pool di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="1609e-417">When hosting multiple websites on a server, we recommend isolating the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="1609e-418">La finestra di dialogo **Aggiungi sito Web** di IIS ha questa configurazione come impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="1609e-418">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="1609e-419">Quando si specifica un valore in **Nome del sito**, il testo viene automaticamente trasferito alla casella di testo **Pool di applicazioni**.</span><span class="sxs-lookup"><span data-stu-id="1609e-419">When a **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="1609e-420">Quando si aggiunge il sito viene creato un nuovo pool di applicazioni con il nome del sito.</span><span class="sxs-lookup"><span data-stu-id="1609e-420">A new app pool is created using the site name when the site is added.</span></span>

::: moniker-end

## <a name="application-pool-identity"></a><span data-ttu-id="1609e-421">Identità del pool di applicazione</span><span class="sxs-lookup"><span data-stu-id="1609e-421">Application Pool Identity</span></span>

<span data-ttu-id="1609e-422">Un account di identità del pool di app consente di eseguire un'app in un account univoco, senza dover creare e gestire domini o account locali.</span><span class="sxs-lookup"><span data-stu-id="1609e-422">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="1609e-423">In IIS 8.0 o versioni successive il processo di lavoro amministrazione IIS (WAS) crea un account virtuale con il nome del nuovo pool di applicazioni ed esegue i processi di lavoro del pool di applicazioni inclusi nell'account per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="1609e-423">On IIS 8.0 or later, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="1609e-424">Nella Console di gestione IIS in **Impostazioni avanzate** per il pool di app verificare che **Identità** sia impostata su **ApplicationPoolIdentity**:</span><span class="sxs-lookup"><span data-stu-id="1609e-424">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![Finestra di dialogo Impostazioni avanzate del pool di applicazione](index/_static/apppool-identity.png)

<span data-ttu-id="1609e-426">Il processo di gestione IIS crea un identificatore sicuro con il nome del pool di app nel sistema di sicurezza di Windows.</span><span class="sxs-lookup"><span data-stu-id="1609e-426">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="1609e-427">Le risorse possono essere protette mediante questa identità,</span><span class="sxs-lookup"><span data-stu-id="1609e-427">Resources can be secured using this identity.</span></span> <span data-ttu-id="1609e-428">che tuttavia non è un account utente reale e non viene visualizzata nella console di gestione utenti di Windows.</span><span class="sxs-lookup"><span data-stu-id="1609e-428">However, this identity isn't a real user account and doesn't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="1609e-429">Se il processo di lavoro IIS richiede l'accesso con privilegi elevati all'app, modificare l'elenco di controllo di accesso (ACL) della directory contenente l'app:</span><span class="sxs-lookup"><span data-stu-id="1609e-429">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="1609e-430">Aprire Esplora risorse, quindi spostarsi nella directory.</span><span class="sxs-lookup"><span data-stu-id="1609e-430">Open Windows Explorer and navigate to the directory.</span></span>

1. <span data-ttu-id="1609e-431">Fare clic con il pulsante destro del mouse sulla directory e scegliere **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="1609e-431">Right-click on the directory and select **Properties**.</span></span>

1. <span data-ttu-id="1609e-432">Nella scheda **Sicurezza** selezionare il pulsante **Modifica** e quindi il pulsante **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="1609e-432">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

1. <span data-ttu-id="1609e-433">Fare clic sul pulsante **Percorsi** e verificare che il sistema sia selezionato.</span><span class="sxs-lookup"><span data-stu-id="1609e-433">Select the **Locations** button and make sure the system is selected.</span></span>

1. <span data-ttu-id="1609e-434">Immettere **IIS AppPool\\<nome_pool_applicazioni>** nell'area **Immettere i nomi degli oggetti da selezionare**.</span><span class="sxs-lookup"><span data-stu-id="1609e-434">Enter **IIS AppPool\\<app_pool_name>** in **Enter the object names to select** area.</span></span> <span data-ttu-id="1609e-435">Fare clic sul pulsante **Controlla nomi**.</span><span class="sxs-lookup"><span data-stu-id="1609e-435">Select the **Check Names** button.</span></span> <span data-ttu-id="1609e-436">Per *DefaultAppPool* controllare i nomi usando **IIS AppPool\DefaultAppPool**.</span><span class="sxs-lookup"><span data-stu-id="1609e-436">For the *DefaultAppPool* check the names using **IIS AppPool\DefaultAppPool**.</span></span> <span data-ttu-id="1609e-437">Quando il pulsante **Controlla nomi** è selezionato, nell'area dei nomi degli oggetti viene indicato un valore di **DefaultAppPool**.</span><span class="sxs-lookup"><span data-stu-id="1609e-437">When the **Check Names** button is selected, a value of **DefaultAppPool** is indicated in the object names area.</span></span> <span data-ttu-id="1609e-438">Non è possibile immettere il nome del pool di applicazioni direttamente nell'area dei nomi degli oggetti.</span><span class="sxs-lookup"><span data-stu-id="1609e-438">It isn't possible to enter the app pool name directly into the object names area.</span></span> <span data-ttu-id="1609e-439">Usare il formato **IIS AppPool\\<nome_pool_applicazioni>** durante il controllo dei nomi degli oggetti.</span><span class="sxs-lookup"><span data-stu-id="1609e-439">Use the **IIS AppPool\\<app_pool_name>** format when checking for the object name.</span></span>

   ![Finestra di dialogo Seleziona utenti o gruppi per la cartella dell'app: il nome del pool di applicazioni "DefaultAppPool" viene aggiunto in coda a "IIS AppPool\" nell'area dei nomi degli oggetti prima di selezionare "Controlla nomi".](index/_static/select-users-or-groups-1.png)

1. <span data-ttu-id="1609e-441">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="1609e-441">Select **OK**.</span></span>

   ![Finestra di dialogo Seleziona utenti o gruppi per la cartella dell'app: dopo aver selezionato "Controlla nomi", il nome dell'oggetto "DefaultAppPool" viene visualizzato nell'area dei nomi degli oggetti.](index/_static/select-users-or-groups-2.png)

1. <span data-ttu-id="1609e-443">Le autorizzazioni di lettura ed esecuzione devono essere concesse per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="1609e-443">Read &amp; execute permissions should be granted by default.</span></span> <span data-ttu-id="1609e-444">Fornire autorizzazioni aggiuntive in base alle necessità.</span><span class="sxs-lookup"><span data-stu-id="1609e-444">Provide additional permissions as needed.</span></span>

<span data-ttu-id="1609e-445">L'accesso può essere concesso anche dal prompt dei comandi usando lo strumento **ICACLS**.</span><span class="sxs-lookup"><span data-stu-id="1609e-445">Access can also be granted at a command prompt using the **ICACLS** tool.</span></span> <span data-ttu-id="1609e-446">Usando *DefaultAppPool* come esempio, viene eseguito il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="1609e-446">Using the *DefaultAppPool* as an example, the following command is used:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

<span data-ttu-id="1609e-447">Per altre informazioni, vedere l'argomento [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="1609e-447">For more information, see the [icacls](/windows-server/administration/windows-commands/icacls) topic.</span></span>

## <a name="http2-support"></a><span data-ttu-id="1609e-448">Supporto per HTTP/2</span><span class="sxs-lookup"><span data-stu-id="1609e-448">HTTP/2 support</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="1609e-449">[HTTP/2](https://httpwg.org/specs/rfc7540.html) è supportato con ASP.NET Core negli scenari di distribuzione IIS seguenti:</span><span class="sxs-lookup"><span data-stu-id="1609e-449">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="1609e-450">In-Process</span><span class="sxs-lookup"><span data-stu-id="1609e-450">In-process</span></span>
  * <span data-ttu-id="1609e-451">Windows Server 2016/Windows 10 o versioni successive; IIS 10 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="1609e-451">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="1609e-452">Connessione TLS 1.2 o successiva</span><span class="sxs-lookup"><span data-stu-id="1609e-452">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="1609e-453">Out-of-process</span><span class="sxs-lookup"><span data-stu-id="1609e-453">Out-of-process</span></span>
  * <span data-ttu-id="1609e-454">Windows Server 2016/Windows 10 o versioni successive; IIS 10 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="1609e-454">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="1609e-455">Le connessioni server perimetrali pubbliche usano HTTP/2, mentre la connessione con proxy inverso al [server Kestrel](xref:fundamentals/servers/kestrel) usa HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="1609e-455">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
  * <span data-ttu-id="1609e-456">Connessione TLS 1.2 o successiva</span><span class="sxs-lookup"><span data-stu-id="1609e-456">TLS 1.2 or later connection</span></span>

<span data-ttu-id="1609e-457">Per una distribuzione In-Process, se viene stabilita una connessione HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) corrisponde a `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="1609e-457">For an in-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="1609e-458">Per una distribuzione out-of-process, se viene stabilita una connessione HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) corrisponde a `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="1609e-458">For an out-of-process deployment when an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="1609e-459">Per altre informazioni sui modelli di hosting in-process e out-of-process, vedere l'argomento <xref:fundamentals/servers/aspnet-core-module> e <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="1609e-459">For more information on the in-process and out-of-process hosting models, see the <xref:fundamentals/servers/aspnet-core-module> topic and the <xref:host-and-deploy/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="1609e-460">[HTTP/2](https://httpwg.org/specs/rfc7540.html) è supportato per le distribuzioni out-of-process che soddisfano i requisiti seguenti:</span><span class="sxs-lookup"><span data-stu-id="1609e-460">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported for out-of-process deployments that meet the following base requirements:</span></span>

* <span data-ttu-id="1609e-461">Windows Server 2016/Windows 10 o versioni successive; IIS 10 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="1609e-461">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
* <span data-ttu-id="1609e-462">Le connessioni server perimetrali pubbliche usano HTTP/2, mentre la connessione con proxy inverso al [server Kestrel](xref:fundamentals/servers/kestrel) usa HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="1609e-462">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>
* <span data-ttu-id="1609e-463">Frame di destinazione: non applicabile alle distribuzioni out-of-process, in quanto la connessione HTTP/2 è gestita interamente da IIS.</span><span class="sxs-lookup"><span data-stu-id="1609e-463">Target framework: Not applicable to out-of-process deployments, since the HTTP/2 connection is handled entirely by IIS.</span></span>
* <span data-ttu-id="1609e-464">Connessione TLS 1.2 o successiva</span><span class="sxs-lookup"><span data-stu-id="1609e-464">TLS 1.2 or later connection</span></span>

<span data-ttu-id="1609e-465">Se viene stabilita una connessione HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) corrisponde a `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="1609e-465">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="1609e-466">HTTP/2 è abilitato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="1609e-466">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="1609e-467">Se non viene stabilita una connessione HTTP/2, la connessione esegue il fallback a HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="1609e-467">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="1609e-468">Per altre informazioni sulla configurazione di HTTP/2 con le distribuzioni IIS, vedere [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis) (HTTP/2 in IIS).</span><span class="sxs-lookup"><span data-stu-id="1609e-468">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="deployment-resources-for-iis-administrators"></a><span data-ttu-id="1609e-469">Risorse di distribuzione per amministratori IIS</span><span class="sxs-lookup"><span data-stu-id="1609e-469">Deployment resources for IIS administrators</span></span>

<span data-ttu-id="1609e-470">Informazioni approfondite su IIS nella documentazione di IIS.</span><span class="sxs-lookup"><span data-stu-id="1609e-470">Learn about IIS in-depth in the IIS documentation.</span></span>  
[<span data-ttu-id="1609e-471">Documentazione di ISS</span><span class="sxs-lookup"><span data-stu-id="1609e-471">IIS documentation</span></span>](/iis)

<span data-ttu-id="1609e-472">Informazioni sui modelli di distribuzione di app .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1609e-472">Learn about .NET Core app deployment models.</span></span>  
[<span data-ttu-id="1609e-473">Distribuzione di applicazioni .NET Core</span><span class="sxs-lookup"><span data-stu-id="1609e-473">.NET Core application deployment</span></span>](/dotnet/core/deploying/)

<span data-ttu-id="1609e-474">Informazioni su come il modulo ASP.NET Core consente al server Web Kestrel di usare IIS o IIS Express come server proxy inverso.</span><span class="sxs-lookup"><span data-stu-id="1609e-474">Learn how the ASP.NET Core Module allows the Kestrel web server to use IIS or IIS Express as a reverse proxy server.</span></span>  
[<span data-ttu-id="1609e-475">Modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1609e-475">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)

<span data-ttu-id="1609e-476">Informazioni su come configurare il modulo di ASP.NET Core per l'hosting di app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1609e-476">Learn how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span>  
[<span data-ttu-id="1609e-477">Guida di riferimento per la configurazione del modulo ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1609e-477">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)

<span data-ttu-id="1609e-478">Informazioni sulla struttura di directory delle app ASP.NET Core pubblicate.</span><span class="sxs-lookup"><span data-stu-id="1609e-478">Learn about the directory structure of published ASP.NET Core apps.</span></span>  
[<span data-ttu-id="1609e-479">Struttura di directory</span><span class="sxs-lookup"><span data-stu-id="1609e-479">Directory structure</span></span>](xref:host-and-deploy/directory-structure)

<span data-ttu-id="1609e-480">Individuare i moduli IIS attivi e inattivi per le app ASP.NET Core e come gestire i moduli IIS.</span><span class="sxs-lookup"><span data-stu-id="1609e-480">Discover active and inactive IIS modules for ASP.NET Core apps and how to manage IIS modules.</span></span>  
[<span data-ttu-id="1609e-481">Moduli IIS</span><span class="sxs-lookup"><span data-stu-id="1609e-481">IIS modules</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="1609e-482">Informazioni su come diagnosticare i problemi delle distribuzioni IIS di app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1609e-482">Learn how to diagnose problems with IIS deployments of ASP.NET Core apps.</span></span>  
[<span data-ttu-id="1609e-483">Risolvere i problemi</span><span class="sxs-lookup"><span data-stu-id="1609e-483">Troubleshoot</span></span>](xref:host-and-deploy/iis/troubleshoot)

<span data-ttu-id="1609e-484">Riconoscere errori comuni dell'hosting di app ASP.NET Core in IIS.</span><span class="sxs-lookup"><span data-stu-id="1609e-484">Distinguish common errors when hosting ASP.NET Core apps on IIS.</span></span>  
[<span data-ttu-id="1609e-485">Errori comuni di Azure App Service e IIS</span><span class="sxs-lookup"><span data-stu-id="1609e-485">Common errors reference for Azure App Service and IIS</span></span>](xref:host-and-deploy/azure-iis-errors-reference)

## <a name="additional-resources"></a><span data-ttu-id="1609e-486">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="1609e-486">Additional resources</span></span>

* [<span data-ttu-id="1609e-487">Introduzione a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1609e-487">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="1609e-488">Il sito IIS ufficiale di Microsoft</span><span class="sxs-lookup"><span data-stu-id="1609e-488">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="1609e-489">Libreria di contenuti tecnici di Windows Server</span><span class="sxs-lookup"><span data-stu-id="1609e-489">Windows Server technical content library</span></span>](/windows-server/windows-server)
* [<span data-ttu-id="1609e-490">HTTP/2 in IIS</span><span class="sxs-lookup"><span data-stu-id="1609e-490">HTTP/2 on IIS</span></span>](/iis/get-started/whats-new-in-iis-10/http2-on-iis)
