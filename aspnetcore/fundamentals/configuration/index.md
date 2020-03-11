---
title: Configurazione in ASP.NET Core
author: guardrex
description: Informazioni su come usare l'API di configurazione per configurare un'app ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/10/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: 3dcabae3f76d81e641057c346dbb9097c2da42c7
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78656331"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="d97f5-103">Configurazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d97f5-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="d97f5-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d97f5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d97f5-105">La configurazione delle app in ASP.NET Core si basa su coppie chiave-valore stabilite dai *provider di configurazione*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="d97f5-106">I provider di configurazione leggono i dati di configurazione in coppie chiave-valore da un'ampia gamma di origini di configurazione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="d97f5-107">Insieme di credenziali chiave di Azure</span><span class="sxs-lookup"><span data-stu-id="d97f5-107">Azure Key Vault</span></span>
* <span data-ttu-id="d97f5-108">Configurazione app di Azure</span><span class="sxs-lookup"><span data-stu-id="d97f5-108">Azure App Configuration</span></span>
* <span data-ttu-id="d97f5-109">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="d97f5-109">Command-line arguments</span></span>
* <span data-ttu-id="d97f5-110">Provider personalizzati (installati o creati)</span><span class="sxs-lookup"><span data-stu-id="d97f5-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="d97f5-111">File della directory</span><span class="sxs-lookup"><span data-stu-id="d97f5-111">Directory files</span></span>
* <span data-ttu-id="d97f5-112">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="d97f5-112">Environment variables</span></span>
* <span data-ttu-id="d97f5-113">Oggetti .NET in memoria</span><span class="sxs-lookup"><span data-stu-id="d97f5-113">In-memory .NET objects</span></span>
* <span data-ttu-id="d97f5-114">File di impostazioni</span><span class="sxs-lookup"><span data-stu-id="d97f5-114">Settings files</span></span>

<span data-ttu-id="d97f5-115">I pacchetti di configurazione per gli scenari di provider di configurazione comuni ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) sono inclusi in modo implicito dal framework.</span><span class="sxs-lookup"><span data-stu-id="d97f5-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

<span data-ttu-id="d97f5-116">Gli esempi di codice che seguono e quelli inclusi nell'app di esempio usano lo spazio dei nomi <xref:Microsoft.Extensions.Configuration>:</span><span class="sxs-lookup"><span data-stu-id="d97f5-116">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="d97f5-117">Il *modello di opzioni* è un'estensione dei concetti di configurazione descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="d97f5-117">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="d97f5-118">Le opzioni usano le classi per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="d97f5-118">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="d97f5-119">Per altre informazioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-119">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="d97f5-120">[Visualizzare o scaricare il codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d97f5-120">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="d97f5-121">Host e configurazione delle app</span><span class="sxs-lookup"><span data-stu-id="d97f5-121">Host versus app configuration</span></span>

<span data-ttu-id="d97f5-122">Prima che l'app venga configurata e avviata, viene configurato e avviato un *host*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-122">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="d97f5-123">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="d97f5-123">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="d97f5-124">Sia l'app che l'host vengono configurati tramite i provider di configurazione descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="d97f5-124">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="d97f5-125">Anche le coppie chiave-valore di configurazione dell'host sono incluse nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="d97f5-125">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="d97f5-126">Per altre informazioni su come vengono usati i provider di configurazione quando viene creato l'host e sugli effetti delle origini di configurazione sull'host di configurazione, vedere <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-126">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="d97f5-127">Altra configurazione</span><span class="sxs-lookup"><span data-stu-id="d97f5-127">Other configuration</span></span>

<span data-ttu-id="d97f5-128">Questo argomento riguarda solo la *configurazione dell'app*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-128">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="d97f5-129">Altri aspetti dell'esecuzione e dell'hosting di app ASP.NET Core sono configurati usando i file di configurazione non trattati in questo argomento:</span><span class="sxs-lookup"><span data-stu-id="d97f5-129">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="d97f5-130">*Launch. json*/*launchSettings. JSON* sono i file di configurazione degli strumenti per l'ambiente di sviluppo, descritti di seguito:</span><span class="sxs-lookup"><span data-stu-id="d97f5-130">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="d97f5-131">In <xref:fundamentals/environments#development>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-131">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="d97f5-132">Nella documentazione in cui vengono usati i file per configurare ASP.NET Core app per scenari di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-132">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="d97f5-133">*Web. config* è un file di configurazione del server, descritto negli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="d97f5-133">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="d97f5-134">Per ulteriori informazioni sulla migrazione della configurazione dell'app da versioni precedenti di ASP.NET, vedere <xref:migration/proper-to-2x/index#store-configurations>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-134">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="d97f5-135">Configurazione predefinita</span><span class="sxs-lookup"><span data-stu-id="d97f5-135">Default configuration</span></span>

<span data-ttu-id="d97f5-136">Le app basate sui modelli [dotnet new](/dotnet/core/tools/dotnet-new) di ASP.NET Core chiamano <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> durante la compilazione di un host.</span><span class="sxs-lookup"><span data-stu-id="d97f5-136">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="d97f5-137">`CreateDefaultBuilder` fornisce la configurazione predefinita per l'app nell'ordine seguente:</span><span class="sxs-lookup"><span data-stu-id="d97f5-137">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="d97f5-138">La configurazione seguente si applica alle app che usano l'[host generico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="d97f5-138">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="d97f5-139">Per informazioni dettagliate sulla configurazione predefinita quando viene usato l'[host Web](xref:fundamentals/host/web-host), vedere la [versione di questo argomento per ASP.NET Core 2.2](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="d97f5-139">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="d97f5-140">La configurazione dell'host viene specificata da:</span><span class="sxs-lookup"><span data-stu-id="d97f5-140">Host configuration is provided from:</span></span>
  * <span data-ttu-id="d97f5-141">Variabili di ambiente con prefisso `DOTNET_` (ad esempio `DOTNET_ENVIRONMENT`) che usano il [provider di configurazione delle variabili di ambiente](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d97f5-141">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="d97f5-142">Il prefisso (`DOTNET_`) viene rimosso al caricamento delle coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-142">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="d97f5-143">Argomenti della riga di comando che usano il [provider di configurazione della riga di comando](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d97f5-143">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="d97f5-144">La configurazione predefinita dell'host Web viene stabilita (`ConfigureWebHostDefaults`) nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="d97f5-144">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="d97f5-145">Kestrel viene usato come server Web e configurato con i provider di configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="d97f5-145">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="d97f5-146">Aggiungere il middleware di filtro host.</span><span class="sxs-lookup"><span data-stu-id="d97f5-146">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="d97f5-147">Aggiungere il middleware delle intestazioni inoltrate se la variabile di ambiente `ASPNETCORE_FORWARDEDHEADERS_ENABLED` è impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-147">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="d97f5-148">Abilitare l'integrazione di IIS.</span><span class="sxs-lookup"><span data-stu-id="d97f5-148">Enable IIS integration.</span></span>
* <span data-ttu-id="d97f5-149">La configurazione dell'app viene fornita da:</span><span class="sxs-lookup"><span data-stu-id="d97f5-149">App configuration is provided from:</span></span>
  * <span data-ttu-id="d97f5-150">*appsettings.json* mediante il [provider di configurazione dei file](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d97f5-150">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="d97f5-151">*appsettings.{Ambiente}.json* mediante il [provider di configurazione dei file](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d97f5-151">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="d97f5-152">[Strumento di gestione dei segreti](xref:security/app-secrets) quando l'app viene eseguita nell'ambiente `Development` usando l'assembly di ingresso.</span><span class="sxs-lookup"><span data-stu-id="d97f5-152">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="d97f5-153">Variabili di ambiente che usano il [provider di configurazione delle variabili di ambiente](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d97f5-153">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="d97f5-154">Argomenti della riga di comando che usano il [provider di configurazione della riga di comando](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d97f5-154">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

## <a name="security"></a><span data-ttu-id="d97f5-155">Security</span><span class="sxs-lookup"><span data-stu-id="d97f5-155">Security</span></span>

<span data-ttu-id="d97f5-156">Per proteggere i dati di configurazione sensibili, adottare le pratiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="d97f5-156">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="d97f5-157">Non archiviare mai la password o altri dati sensibili nel codice del provider di configurazione o in file di configurazione di testo normale.</span><span class="sxs-lookup"><span data-stu-id="d97f5-157">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="d97f5-158">Non usare i segreti di produzione in ambienti di sviluppo o di test.</span><span class="sxs-lookup"><span data-stu-id="d97f5-158">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="d97f5-159">Specificare i segreti all'esterno del progetto in modo che non possano essere inavvertitamente inviati a un repository del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="d97f5-159">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="d97f5-160">Per altre informazioni, vedere gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="d97f5-160">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="d97f5-161"><xref:security/app-secrets> &ndash; include consigli sull'utilizzo delle variabili di ambiente per l'archiviazione di dati sensibili.</span><span class="sxs-lookup"><span data-stu-id="d97f5-161"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="d97f5-162">Secret Manager usa il provider di configurazione dei file per archiviare i segreti utente in un file JSON nel sistema locale.</span><span class="sxs-lookup"><span data-stu-id="d97f5-162">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="d97f5-163">Il provider di configurazione dei file è descritto più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="d97f5-163">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="d97f5-164">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) archivia in modo sicuro i segreti delle app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d97f5-164">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="d97f5-165">Per altre informazioni, vedere <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-165">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="d97f5-166">Dati di configurazione gerarchici</span><span class="sxs-lookup"><span data-stu-id="d97f5-166">Hierarchical configuration data</span></span>

<span data-ttu-id="d97f5-167">L'API di configurazione è in grado di mantenere i dati di configurazione gerarchici rendendoli flat grazie all'uso di un delimitatore nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-167">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="d97f5-168">Nel file JSON seguente, esistono quattro chiavi in una gerarchia strutturata di due sezioni:</span><span class="sxs-lookup"><span data-stu-id="d97f5-168">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

<span data-ttu-id="d97f5-169">Quando il file viene letto nella configurazione, vengono create chiavi univoche per mantenere la struttura di dati gerarchici originale dell'origine di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-169">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="d97f5-170">Le sezioni e le chiavi vengono rese flat usando due punti (`:`) per mantenere la struttura originale:</span><span class="sxs-lookup"><span data-stu-id="d97f5-170">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="d97f5-171">section0:key0</span><span class="sxs-lookup"><span data-stu-id="d97f5-171">section0:key0</span></span>
* <span data-ttu-id="d97f5-172">section0:key1</span><span class="sxs-lookup"><span data-stu-id="d97f5-172">section0:key1</span></span>
* <span data-ttu-id="d97f5-173">section1:key0</span><span class="sxs-lookup"><span data-stu-id="d97f5-173">section1:key0</span></span>
* <span data-ttu-id="d97f5-174">section1:key1</span><span class="sxs-lookup"><span data-stu-id="d97f5-174">section1:key1</span></span>

<span data-ttu-id="d97f5-175">I metodi <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> e <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> sono disponibili per isolare le sezioni e gli elementi figlio di una sezione nei dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-175"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="d97f5-176">Questi metodi sono descritti più avanti in [GetSection, GetChildren ed Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="d97f5-176">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="d97f5-177">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="d97f5-177">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="d97f5-178">Origini e provider</span><span class="sxs-lookup"><span data-stu-id="d97f5-178">Sources and providers</span></span>

<span data-ttu-id="d97f5-179">All'avvio dell'app, le origini di configurazione vengono lette nell'ordine con cui sono specificati i rispettivi provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-179">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="d97f5-180">I provider di configurazione che implementano il rilevamento delle modifiche sono in grado di ricaricare la configurazione quando viene modificata un'impostazione sottostante.</span><span class="sxs-lookup"><span data-stu-id="d97f5-180">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="d97f5-181">Ad esempio, il provider di configurazione dei file (descritto più avanti in questo argomento) e il [provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration) implementano il rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="d97f5-181">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="d97f5-182"><xref:Microsoft.Extensions.Configuration.IConfiguration> è disponibile nel contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) dell'app.</span><span class="sxs-lookup"><span data-stu-id="d97f5-182"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="d97f5-183"><xref:Microsoft.Extensions.Configuration.IConfiguration> possono essere inseriti in un Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> o MVC <xref:Microsoft.AspNetCore.Mvc.Controller> per ottenere la configurazione per la classe.</span><span class="sxs-lookup"><span data-stu-id="d97f5-183"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="d97f5-184">Negli esempi seguenti viene usato il campo `_config` per accedere ai valori di configurazione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-184">In the following examples, the `_config` field is used to access configuration values:</span></span>

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }
}
```

```csharp
public class HomeController : Controller
{
    private readonly IConfiguration _config;

    public HomeController(IConfiguration config)
    {
        _config = config;
    }
}
```

<span data-ttu-id="d97f5-185">I provider di configurazione non possono usare l'inserimento delle dipendenze, perché non è disponibile quando vengono configurati dall'host.</span><span class="sxs-lookup"><span data-stu-id="d97f5-185">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="d97f5-186">Chiavi</span><span class="sxs-lookup"><span data-stu-id="d97f5-186">Keys</span></span>

<span data-ttu-id="d97f5-187">Le chiavi di configurazione adottano le convenzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="d97f5-187">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="d97f5-188">Per le chiavi non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="d97f5-188">Keys are case-insensitive.</span></span> <span data-ttu-id="d97f5-189">Ad esempio, `ConnectionString` e `connectionstring` vengono considerate chiavi equivalenti.</span><span class="sxs-lookup"><span data-stu-id="d97f5-189">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="d97f5-190">Se un valore per la stessa chiave viene impostato dallo stesso provider di configurazione o da provider diversi, viene usato l'ultimo valore impostato per la chiave.</span><span class="sxs-lookup"><span data-stu-id="d97f5-190">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="d97f5-191">Chiavi gerarchiche</span><span class="sxs-lookup"><span data-stu-id="d97f5-191">Hierarchical keys</span></span>
  * <span data-ttu-id="d97f5-192">Nell'ambito dell'API di configurazione, il separatore due punti (`:`) funziona in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="d97f5-192">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="d97f5-193">Nelle variabili di ambiente, un separatore due punti potrebbe non funzionare in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="d97f5-193">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="d97f5-194">Il doppio carattere di sottolineatura (`__`) è supportato da tutte le piattaforme e viene convertito automaticamente nei due punti.</span><span class="sxs-lookup"><span data-stu-id="d97f5-194">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="d97f5-195">In Azure Key Vault, le chiavi gerarchiche usano `--` (due trattini) come separatore.</span><span class="sxs-lookup"><span data-stu-id="d97f5-195">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="d97f5-196">Scrivere il codice per sostituire i trattini con i due punti quando i segreti vengono caricati nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="d97f5-196">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="d97f5-197">Il <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supporta l'associazione di matrici agli oggetti usando gli indici delle matrici nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-197">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="d97f5-198">L'associazione di matrici è descritta nella sezione [Associare una matrice a una classe](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="d97f5-198">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="d97f5-199">Valori</span><span class="sxs-lookup"><span data-stu-id="d97f5-199">Values</span></span>

<span data-ttu-id="d97f5-200">I valori di configurazione adottano le convenzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="d97f5-200">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="d97f5-201">I valori sono stringhe.</span><span class="sxs-lookup"><span data-stu-id="d97f5-201">Values are strings.</span></span>
* <span data-ttu-id="d97f5-202">I valori null non possono essere archiviati nella configurazione o associati a oggetti.</span><span class="sxs-lookup"><span data-stu-id="d97f5-202">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="d97f5-203">Providers</span><span class="sxs-lookup"><span data-stu-id="d97f5-203">Providers</span></span>

<span data-ttu-id="d97f5-204">La tabella seguente mostra i provider di configurazione disponibili per le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d97f5-204">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="d97f5-205">Provider</span><span class="sxs-lookup"><span data-stu-id="d97f5-205">Provider</span></span> | <span data-ttu-id="d97f5-206">Fornisce la configurazione da&hellip;</span><span class="sxs-lookup"><span data-stu-id="d97f5-206">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="d97f5-207">[Provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="d97f5-207">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="d97f5-208">Insieme di credenziali chiave di Azure</span><span class="sxs-lookup"><span data-stu-id="d97f5-208">Azure Key Vault</span></span> |
| <span data-ttu-id="d97f5-209">[Provider di Configurazione app](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Documentazione di Azure)</span><span class="sxs-lookup"><span data-stu-id="d97f5-209">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="d97f5-210">Configurazione app di Azure</span><span class="sxs-lookup"><span data-stu-id="d97f5-210">Azure App Configuration</span></span> |
| [<span data-ttu-id="d97f5-211">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="d97f5-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="d97f5-212">Parametri della riga di comando</span><span class="sxs-lookup"><span data-stu-id="d97f5-212">Command-line parameters</span></span> |
| [<span data-ttu-id="d97f5-213">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="d97f5-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="d97f5-214">Origine personalizzata</span><span class="sxs-lookup"><span data-stu-id="d97f5-214">Custom source</span></span> |
| [<span data-ttu-id="d97f5-215">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="d97f5-215">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="d97f5-216">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="d97f5-216">Environment variables</span></span> |
| [<span data-ttu-id="d97f5-217">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="d97f5-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="d97f5-218">File (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="d97f5-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="d97f5-219">Provider di configurazione chiave-per-file</span><span class="sxs-lookup"><span data-stu-id="d97f5-219">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="d97f5-220">File della directory</span><span class="sxs-lookup"><span data-stu-id="d97f5-220">Directory files</span></span> |
| [<span data-ttu-id="d97f5-221">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="d97f5-221">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="d97f5-222">Raccolte in memoria</span><span class="sxs-lookup"><span data-stu-id="d97f5-222">In-memory collections</span></span> |
| <span data-ttu-id="d97f5-223">[Segreti utente (Secret Manager)](xref:security/app-secrets) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="d97f5-223">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="d97f5-224">File nella directory dei profili utente</span><span class="sxs-lookup"><span data-stu-id="d97f5-224">File in the user profile directory</span></span> |

<span data-ttu-id="d97f5-225">Le origini di configurazione vengono lette nell'ordine in cui vengono specificati i rispetti provider di configurazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="d97f5-225">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="d97f5-226">I provider di configurazione descritti in questo argomento sono descritti in ordine alfabetico, non nell'ordine in cui il codice li dispone.</span><span class="sxs-lookup"><span data-stu-id="d97f5-226">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="d97f5-227">Ordinare i provider di configurazione nel codice in base alle priorità per le origini di configurazione sottostanti richieste dall'app.</span><span class="sxs-lookup"><span data-stu-id="d97f5-227">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="d97f5-228">Una sequenza tipica di provider di configurazione è:</span><span class="sxs-lookup"><span data-stu-id="d97f5-228">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="d97f5-229">File (*appsettings.json*, *appsettings.{Environment}.json*, dove `{Environment}` è l'ambiente di hosting corrente dell'app)</span><span class="sxs-lookup"><span data-stu-id="d97f5-229">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="d97f5-230">Insieme di credenziali chiave Azure</span><span class="sxs-lookup"><span data-stu-id="d97f5-230">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="d97f5-231">[Segreti utente (Secret Manager)](xref:security/app-secrets) (solo nell'ambiente di sviluppo)</span><span class="sxs-lookup"><span data-stu-id="d97f5-231">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="d97f5-232">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="d97f5-232">Environment variables</span></span>
1. <span data-ttu-id="d97f5-233">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="d97f5-233">Command-line arguments</span></span>

<span data-ttu-id="d97f5-234">È pratica comune posizionare il provider di configurazione della riga di comando per ultimo in una serie di provider per consentire agli argomenti della riga di comando di sostituire la configurazione impostata da altri provider.</span><span class="sxs-lookup"><span data-stu-id="d97f5-234">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="d97f5-235">La sequenza di provider precedente viene utilizzata quando un nuovo generatore host viene inizializzato con `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-235">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="d97f5-236">Per altre informazioni, vedere la sezione [Configurazione predefinita](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="d97f5-236">For more information, see the [Default configuration](#default-configuration) section.</span></span>

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="d97f5-237">Configurare il generatore di host con ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="d97f5-237">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="d97f5-238">Per configurare il generatore di host, chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> e specificare la configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-238">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="d97f5-239">`ConfigureHostConfiguration` viene usato per inizializzare l'oggetto <xref:Microsoft.Extensions.Hosting.IHostEnvironment> da usare in un secondo momento nel processo di compilazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-239">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="d97f5-240">`ConfigureHostConfiguration` può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="d97f5-240">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureHostConfiguration(config =>
        {
            var dict = new Dictionary<string, string>
            {
                {"MemoryCollectionKey1", "value1"},
                {"MemoryCollectionKey2", "value2"}
            };

            config.AddInMemoryCollection(dict);
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

## <a name="configureappconfiguration"></a><span data-ttu-id="d97f5-241">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="d97f5-241">ConfigureAppConfiguration</span></span>

<span data-ttu-id="d97f5-242">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare i provider di configurazione dell'app, oltre a quelli aggiunti automaticamente da `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d97f5-242">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="d97f5-243">Sostituire la configurazione precedente con argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="d97f5-243">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="d97f5-244">Per fornire la configurazione dell'app che può essere sostituita con argomenti della riga di comando, chiamare `AddCommandLine` per ultimo:</span><span class="sxs-lookup"><span data-stu-id="d97f5-244">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="d97f5-245">Rimuovere i provider aggiunti da CreateDefaultBuilder</span><span class="sxs-lookup"><span data-stu-id="d97f5-245">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="d97f5-246">Per rimuovere i provider aggiunti da `CreateDefaultBuilder`, chiamare prima [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) in [IConfigurationBuilder. Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) :</span><span class="sxs-lookup"><span data-stu-id="d97f5-246">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="d97f5-247">Utilizzare la configurazione durante l'avvio dell'app</span><span class="sxs-lookup"><span data-stu-id="d97f5-247">Consume configuration during app startup</span></span>

<span data-ttu-id="d97f5-248">La configurazione fornita all'app in `ConfigureAppConfiguration` è disponibile durante l'avvio dell'app, incluso `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-248">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d97f5-249">Per altre informazioni, vedere la sezione [Accedere alla configurazione durante l'avvio](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="d97f5-249">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="d97f5-250">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="d97f5-250">Command-line Configuration Provider</span></span>

<span data-ttu-id="d97f5-251"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> carica la configurazione da coppie chiave-valore di argomenti della riga di comando in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-251">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="d97f5-252">Per attivare la configurazione della riga di comando, metodo di estensione <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> viene chiamato su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-252">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d97f5-253">`AddCommandLine` viene chiamato automaticamente quando viene chiamato il metodo `CreateDefaultBuilder(string [])`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-253">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="d97f5-254">Per altre informazioni, vedere la sezione [Configurazione predefinita](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="d97f5-254">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="d97f5-255">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="d97f5-255">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="d97f5-256">Una configurazione facoltativa dai file *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-256">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="d97f5-257">[Segreti utente (Secret Manager)](xref:security/app-secrets) nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-257">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="d97f5-258">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="d97f5-258">Environment variables.</span></span>

<span data-ttu-id="d97f5-259">`CreateDefaultBuilder` aggiunge il provider di configurazione della riga di comando per ultimo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-259">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="d97f5-260">Gli argomenti della riga di comando passati in fase di esecuzione sostituiscono la configurazione impostata dagli altri provider.</span><span class="sxs-lookup"><span data-stu-id="d97f5-260">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="d97f5-261">`CreateDefaultBuilder` opera durante la creazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="d97f5-261">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="d97f5-262">Pertanto, la configurazione della riga di comando attivata da `CreateDefaultBuilder` può influire sul modo in cui l'host viene configurato.</span><span class="sxs-lookup"><span data-stu-id="d97f5-262">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="d97f5-263">Per le app basate sui modelli di ASP.NET Core, `AddCommandLine` è già stato chiamato da `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-263">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="d97f5-264">Per aggiungere altri provider di configurazione e mantenere la possibilità di sostituire la configurazione di tali provider con argomenti della riga di comando, chiamare i provider aggiuntivi dell'app in `ConfigureAppConfiguration` e quindi chiamare `AddCommandLine` per ultimo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-264">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="d97f5-265">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="d97f5-265">**Example**</span></span>

<span data-ttu-id="d97f5-266">L'app di esempio consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include una chiamata a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-266">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="d97f5-267">Aprire un prompt dei comandi nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="d97f5-267">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="d97f5-268">Fornire un argomento della riga di comando per il comando `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-268">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="d97f5-269">Dopo che l'app è in esecuzione, aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-269">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="d97f5-270">Notare che l'output contiene la coppia chiave-valore per l'argomento della riga di comando di configurazione fornito per `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-270">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="d97f5-271">Argomenti</span><span class="sxs-lookup"><span data-stu-id="d97f5-271">Arguments</span></span>

<span data-ttu-id="d97f5-272">Il valore deve seguire un segno di uguale (`=`) o la chiave deve avere un prefisso (`--` o `/`) quando il valore segue uno spazio.</span><span class="sxs-lookup"><span data-stu-id="d97f5-272">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="d97f5-273">Il valore non è necessario se viene usato un segno di uguale, ad esempio `CommandLineKey=`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-273">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="d97f5-274">Prefisso chiave</span><span class="sxs-lookup"><span data-stu-id="d97f5-274">Key prefix</span></span>               | <span data-ttu-id="d97f5-275">Esempio</span><span class="sxs-lookup"><span data-stu-id="d97f5-275">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="d97f5-276">Nessun prefisso</span><span class="sxs-lookup"><span data-stu-id="d97f5-276">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="d97f5-277">Due trattini (`--`)</span><span class="sxs-lookup"><span data-stu-id="d97f5-277">Two dashes (`--`)</span></span>        | <span data-ttu-id="d97f5-278">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="d97f5-278">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="d97f5-279">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="d97f5-279">Forward slash (`/`)</span></span>      | <span data-ttu-id="d97f5-280">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="d97f5-280">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="d97f5-281">Nello stesso comando, non mischiare coppie chiave-valore di argomenti della riga di comando che usano un segno di uguale con coppie chiave-valore che usano uno spazio.</span><span class="sxs-lookup"><span data-stu-id="d97f5-281">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="d97f5-282">Comandi di esempio:</span><span class="sxs-lookup"><span data-stu-id="d97f5-282">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="d97f5-283">Mapping di sostituzione</span><span class="sxs-lookup"><span data-stu-id="d97f5-283">Switch mappings</span></span>

<span data-ttu-id="d97f5-284">I mapping di sostituzione consentono di usare la logica di sostituzione del nome della chiave.</span><span class="sxs-lookup"><span data-stu-id="d97f5-284">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="d97f5-285">Quando si compila manualmente la configurazione con una <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, fornire un dizionario di sostituzioni switch al metodo <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-285">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="d97f5-286">Quando viene utilizzato il dizionario dei mapping di sostituzione, nel dizionario viene controllata la presenza di una chiave corrispondente alla chiave fornita da un argomento della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="d97f5-286">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="d97f5-287">Se la chiave della riga di comando viene trovata nel dizionario, il valore del dizionario (la sostituzione della chiave) viene passato nuovamente per impostare la coppia chiave-valore nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="d97f5-287">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="d97f5-288">Un mapping di sostituzione è necessario per le chiavi della riga di comando con un trattino singolo (`-`) come prefisso.</span><span class="sxs-lookup"><span data-stu-id="d97f5-288">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="d97f5-289">Regole principali del dizionario dei mapping di sostituzione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-289">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="d97f5-290">Le sostituzioni devono iniziare con un trattino (`-`) o un trattino doppio (`--`).</span><span class="sxs-lookup"><span data-stu-id="d97f5-290">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="d97f5-291">Il dizionario dei mapping di sostituzione non deve contenere chiavi duplicate.</span><span class="sxs-lookup"><span data-stu-id="d97f5-291">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="d97f5-292">Creare un dizionario dei mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-292">Create a switch mappings dictionary.</span></span> <span data-ttu-id="d97f5-293">Nell'esempio seguente vengono creati due mapping di sostituzione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-293">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="d97f5-294">Quando viene creato l'host, chiamare `AddCommandLine` con il dizionario dei mapping di sostituzione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-294">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="d97f5-295">Per le app che usano i mapping di sostituzione, la chiamata a `CreateDefaultBuilder` non deve passare argomenti.</span><span class="sxs-lookup"><span data-stu-id="d97f5-295">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="d97f5-296">La chiamata `CreateDefaultBuilder` del metodo `AddCommandLine` non include sostituzioni mappate e non è possibile passare il dizionario dei mapping di sostituzione a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-296">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="d97f5-297">La soluzione consiste nel non passare gli argomenti a `CreateDefaultBuilder` consentendo invece al metodo `ConfigurationBuilder` del metodo `AddCommandLine` di elaborare entrambi gli argomenti e il dizionario dei mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-297">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="d97f5-298">Il dizionario dei mapping di sostituzione creato contiene i dati visualizzati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="d97f5-298">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="d97f5-299">Chiave</span><span class="sxs-lookup"><span data-stu-id="d97f5-299">Key</span></span>       | <span data-ttu-id="d97f5-300">valore</span><span class="sxs-lookup"><span data-stu-id="d97f5-300">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="d97f5-301">Se le chiavi con mapping di sostituzione vengono usate all'avvio dell'app, la configurazione riceve il valore di configurazione per la chiave fornita dal dizionario:</span><span class="sxs-lookup"><span data-stu-id="d97f5-301">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="d97f5-302">Dopo aver eseguito il comando precedente, la configurazione contiene i valori mostrati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="d97f5-302">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="d97f5-303">Chiave</span><span class="sxs-lookup"><span data-stu-id="d97f5-303">Key</span></span>               | <span data-ttu-id="d97f5-304">valore</span><span class="sxs-lookup"><span data-stu-id="d97f5-304">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="d97f5-305">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="d97f5-305">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="d97f5-306"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> carica la configurazione da coppie chiave-valore di variabili di ambiente in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-306">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="d97f5-307">Per attivare la configurazione delle variabili di ambiente, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-307">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="d97f5-308">[App Azure servizio](https://azure.microsoft.com/services/app-service/) consente di impostare le variabili di ambiente nel portale di Azure che possono eseguire l'override della configurazione dell'app usando il provider di configurazione delle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="d97f5-308">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="d97f5-309">Per altre informazioni, vedere [App di Azure: Eseguire l'override della configurazione delle app usando il portale di Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="d97f5-309">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="d97f5-310">`AddEnvironmentVariables` consente di caricare variabili di ambiente con prefisso `DOTNET_` per la [configurazione host](#host-versus-app-configuration) quando viene inizializzato un nuovo generatore di host con l'[host generico](xref:fundamentals/host/generic-host) e viene chiamato `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-310">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="d97f5-311">Per altre informazioni, vedere la sezione [Configurazione predefinita](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="d97f5-311">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="d97f5-312">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="d97f5-312">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="d97f5-313">Configurazione delle app dalle variabili di ambiente senza prefisso chiamando `AddEnvironmentVariables` senza prefisso.</span><span class="sxs-lookup"><span data-stu-id="d97f5-313">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="d97f5-314">Una configurazione facoltativa dai file *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-314">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="d97f5-315">[Segreti utente (Secret Manager)](xref:security/app-secrets) nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-315">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="d97f5-316">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="d97f5-316">Command-line arguments.</span></span>

<span data-ttu-id="d97f5-317">Il provider di configurazione delle variabili di ambiente viene chiamato dopo aver stabilito la configurazione dai segreti utente e dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-317">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="d97f5-318">La chiamata del provider in questa posizione consente alle variabili di ambiente lette in fase di esecuzione di sostituire la configurazione impostata dai segreti utente e dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-318">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="d97f5-319">Per fornire la configurazione dell'app da variabili di ambiente aggiuntive, chiamare i provider aggiuntivi dell'app in `ConfigureAppConfiguration` e chiamare `AddEnvironmentVariables` con il prefisso:</span><span class="sxs-lookup"><span data-stu-id="d97f5-319">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="d97f5-320">Chiamare `AddEnvironmentVariables` ultimo per consentire alle variabili di ambiente con il prefisso specificato di eseguire l'override dei valori di altri provider.</span><span class="sxs-lookup"><span data-stu-id="d97f5-320">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="d97f5-321">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="d97f5-321">**Example**</span></span>

<span data-ttu-id="d97f5-322">L'app di esempio consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include una chiamata a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-322">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="d97f5-323">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d97f5-323">Run the sample app.</span></span> <span data-ttu-id="d97f5-324">Aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-324">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="d97f5-325">Notare che l'output contiene la coppia chiave-valore per la variabile di ambiente `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-325">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="d97f5-326">Il valore riflette l'ambiente in cui l'app è in esecuzione, in genere `Development` durante l'esecuzione locale.</span><span class="sxs-lookup"><span data-stu-id="d97f5-326">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="d97f5-327">Per limitare l'elenco delle variabili di ambiente restituito dall'app, l'app filtra le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="d97f5-327">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="d97f5-328">Vedere il file *Pages/Index.cshtml.cs* dell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d97f5-328">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="d97f5-329">Per esporre tutte le variabili di ambiente disponibili per l'app, modificare il `FilteredConfiguration` in *pages/index. cshtml. cs* come segue:</span><span class="sxs-lookup"><span data-stu-id="d97f5-329">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="d97f5-330">Prefissi</span><span class="sxs-lookup"><span data-stu-id="d97f5-330">Prefixes</span></span>

<span data-ttu-id="d97f5-331">Le variabili di ambiente caricate nella configurazione dell'app vengono filtrate quando si specifica un prefisso per il metodo `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-331">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="d97f5-332">Ad esempio, per filtrare le variabili di ambiente in base al prefisso `CUSTOM_`, fornire il prefisso al provider di configurazione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-332">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="d97f5-333">Il prefisso viene rimosso quando vengono create le coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-333">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="d97f5-334">Quando viene creato il generatore di host, la configurazione dell'host viene fornita dalle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="d97f5-334">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="d97f5-335">Per altre informazioni sul prefisso usato per queste variabili di ambiente, vedere la sezione [Configurazione predefinita](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="d97f5-335">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="d97f5-336">**Prefissi della stringa di connessione**</span><span class="sxs-lookup"><span data-stu-id="d97f5-336">**Connection string prefixes**</span></span>

<span data-ttu-id="d97f5-337">L'API di configurazione prevede regole di elaborazione speciali per quattro variabili di ambiente di stringa di connessione coinvolte nella configurazione di stringhe di connessione di Azure per l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="d97f5-337">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="d97f5-338">Le variabili di ambiente con i prefissi indicati nella tabella vengono caricate nell'app se non viene fornito alcun prefisso a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-338">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="d97f5-339">Prefisso della stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="d97f5-339">Connection string prefix</span></span> | <span data-ttu-id="d97f5-340">Provider</span><span class="sxs-lookup"><span data-stu-id="d97f5-340">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="d97f5-341">Provider personalizzato</span><span class="sxs-lookup"><span data-stu-id="d97f5-341">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="d97f5-342">MySQL</span><span class="sxs-lookup"><span data-stu-id="d97f5-342">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="d97f5-343">Database SQL di Azure</span><span class="sxs-lookup"><span data-stu-id="d97f5-343">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="d97f5-344">SQL Server</span><span class="sxs-lookup"><span data-stu-id="d97f5-344">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="d97f5-345">Quando una variabile di ambiente viene individuata e caricata nella configurazione con uno qualsiasi dei quattro prefissi indicati nella tabella:</span><span class="sxs-lookup"><span data-stu-id="d97f5-345">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="d97f5-346">La chiave di configurazione viene creata rimuovendo il prefisso della variabile di ambiente e aggiungendo una sezione per la chiave di configurazione (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="d97f5-346">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="d97f5-347">Viene creata una nuova coppia chiave-valore della configurazione che rappresenta il provider di connessione al database (ad eccezione di `CUSTOMCONNSTR_`, che non ha un provider dichiarato).</span><span class="sxs-lookup"><span data-stu-id="d97f5-347">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="d97f5-348">Chiave della variabile di ambiente</span><span class="sxs-lookup"><span data-stu-id="d97f5-348">Environment variable key</span></span> | <span data-ttu-id="d97f5-349">Chiave di configurazione convertita</span><span class="sxs-lookup"><span data-stu-id="d97f5-349">Converted configuration key</span></span> | <span data-ttu-id="d97f5-350">Voce di configurazione del provider</span><span class="sxs-lookup"><span data-stu-id="d97f5-350">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="d97f5-351">Voce di configurazione non creata.</span><span class="sxs-lookup"><span data-stu-id="d97f5-351">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="d97f5-352">Chiave: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="d97f5-352">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="d97f5-353">Valore: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="d97f5-353">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="d97f5-354">Chiave: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="d97f5-354">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="d97f5-355">Valore: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="d97f5-355">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="d97f5-356">Chiave: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="d97f5-356">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="d97f5-357">Valore: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="d97f5-357">Value: `System.Data.SqlClient`</span></span>  |

<span data-ttu-id="d97f5-358">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="d97f5-358">**Example**</span></span>

<span data-ttu-id="d97f5-359">Nel server viene creata una variabile di ambiente della stringa di connessione personalizzata:</span><span class="sxs-lookup"><span data-stu-id="d97f5-359">A custom connection string environment variable is created on the server:</span></span>

* <span data-ttu-id="d97f5-360">Nome &ndash; `CUSTOMCONNSTR_ReleaseDB`</span><span class="sxs-lookup"><span data-stu-id="d97f5-360">Name &ndash; `CUSTOMCONNSTR_ReleaseDB`</span></span>
* <span data-ttu-id="d97f5-361">Valore &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span><span class="sxs-lookup"><span data-stu-id="d97f5-361">Value &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span></span>

<span data-ttu-id="d97f5-362">Se `IConfiguration` viene inserito e assegnato a un campo denominato `_config`, leggere il valore:</span><span class="sxs-lookup"><span data-stu-id="d97f5-362">If `IConfiguration` is injected and assigned to a field named `_config`, read the value:</span></span>

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a><span data-ttu-id="d97f5-363">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="d97f5-363">File Configuration Provider</span></span>

<span data-ttu-id="d97f5-364"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> è la classe base per il caricamento della configurazione dal file system.</span><span class="sxs-lookup"><span data-stu-id="d97f5-364"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="d97f5-365">I provider di configurazione seguenti sono dedicati per specifici tipi di file:</span><span class="sxs-lookup"><span data-stu-id="d97f5-365">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="d97f5-366">Provider di configurazione INI</span><span class="sxs-lookup"><span data-stu-id="d97f5-366">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="d97f5-367">Provider di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="d97f5-367">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="d97f5-368">Provider di configurazione XML</span><span class="sxs-lookup"><span data-stu-id="d97f5-368">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="d97f5-369">Provider di configurazione INI</span><span class="sxs-lookup"><span data-stu-id="d97f5-369">INI Configuration Provider</span></span>

<span data-ttu-id="d97f5-370"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> carica la configurazione da coppie chiave-valore di file INI in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-370">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="d97f5-371">Per attivare la configurazione di file INI, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-371">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d97f5-372">I due punti possono essere usati come delimitatore di sezione nella configurazione di file INI.</span><span class="sxs-lookup"><span data-stu-id="d97f5-372">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="d97f5-373">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="d97f5-373">Overloads permit specifying:</span></span>

* <span data-ttu-id="d97f5-374">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-374">Whether the file is optional.</span></span>
* <span data-ttu-id="d97f5-375">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="d97f5-375">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="d97f5-376">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="d97f5-376">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="d97f5-377">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="d97f5-377">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="d97f5-378">Un esempio generico di un file di configurazione INI:</span><span class="sxs-lookup"><span data-stu-id="d97f5-378">A generic example of an INI configuration file:</span></span>

```ini
[section0]
key0=value
key1=value

[section1]
subsection:key=value

[section2:subsection0]
key=value

[section2:subsection1]
key=value
```

<span data-ttu-id="d97f5-379">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="d97f5-379">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="d97f5-380">section0:key0</span><span class="sxs-lookup"><span data-stu-id="d97f5-380">section0:key0</span></span>
* <span data-ttu-id="d97f5-381">section0:key1</span><span class="sxs-lookup"><span data-stu-id="d97f5-381">section0:key1</span></span>
* <span data-ttu-id="d97f5-382">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="d97f5-382">section1:subsection:key</span></span>
* <span data-ttu-id="d97f5-383">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="d97f5-383">section2:subsection0:key</span></span>
* <span data-ttu-id="d97f5-384">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="d97f5-384">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="d97f5-385">Provider di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="d97f5-385">JSON Configuration Provider</span></span>

<span data-ttu-id="d97f5-386">Il <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> carica la configurazione da coppie chiave-valore di file JSON in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-386">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="d97f5-387">Per attivare la configurazione di file JSON, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-387">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d97f5-388">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="d97f5-388">Overloads permit specifying:</span></span>

* <span data-ttu-id="d97f5-389">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-389">Whether the file is optional.</span></span>
* <span data-ttu-id="d97f5-390">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="d97f5-390">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="d97f5-391">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="d97f5-391">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="d97f5-392">`AddJsonFile` viene chiamato automaticamente due volte quando un nuovo generatore host viene inizializzato con `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-392">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="d97f5-393">Il metodo viene chiamato per caricare la configurazione da:</span><span class="sxs-lookup"><span data-stu-id="d97f5-393">The method is called to load configuration from:</span></span>

* <span data-ttu-id="d97f5-394">*appSettings. json* &ndash; questo file viene letto per primo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-394">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="d97f5-395">La versione dell'ambiente del file può sostituire i valori forniti dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-395">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="d97f5-396">*appSettings. {Environment}. JSON* &ndash; la versione dell'ambiente del file viene caricata in base a [IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="d97f5-396">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="d97f5-397">Per altre informazioni, vedere la sezione [Configurazione predefinita](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="d97f5-397">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="d97f5-398">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="d97f5-398">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="d97f5-399">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="d97f5-399">Environment variables.</span></span>
* <span data-ttu-id="d97f5-400">[Segreti utente (Secret Manager)](xref:security/app-secrets) nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-400">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="d97f5-401">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="d97f5-401">Command-line arguments.</span></span>

<span data-ttu-id="d97f5-402">Il provider di configurazione JSON viene stabilito per primo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-402">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="d97f5-403">I segreti utente, le variabili di ambiente e gli argomenti della riga di comando sostituiscono quindi la configurazione impostata dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-403">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="d97f5-404">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare la configurazione dell'app per file diversi da *appsettings.json* e *appsettings.{Environment}.json*:</span><span class="sxs-lookup"><span data-stu-id="d97f5-404">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="d97f5-405">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="d97f5-405">**Example**</span></span>

<span data-ttu-id="d97f5-406">L'app di esempio sfrutta il metodo di convenienza statica `CreateDefaultBuilder` per compilare l'host, che include due chiamate al `AddJsonFile`:</span><span class="sxs-lookup"><span data-stu-id="d97f5-406">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

* <span data-ttu-id="d97f5-407">La prima chiamata a `AddJsonFile` carica la configurazione da *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="d97f5-407">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="d97f5-408">La seconda chiamata a `AddJsonFile` carica la configurazione da *appSettings. { Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-408">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="d97f5-409">Per *appSettings. Development. JSON* nell'app di esempio, viene caricato il file seguente:</span><span class="sxs-lookup"><span data-stu-id="d97f5-409">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="d97f5-410">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d97f5-410">Run the sample app.</span></span> <span data-ttu-id="d97f5-411">Aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-411">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="d97f5-412">L'output contiene coppie chiave-valore per la configurazione basata sull'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="d97f5-412">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="d97f5-413">Il livello di registrazione per la chiave `Logging:LogLevel:Default` viene `Debug` quando si esegue l'app nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-413">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="d97f5-414">Eseguire di nuovo l'app di esempio nell'ambiente di produzione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-414">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="d97f5-415">Aprire il file *Properties/launchSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="d97f5-415">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="d97f5-416">Nel profilo di `ConfigurationSample` modificare il valore della variabile di ambiente `ASPNETCORE_ENVIRONMENT` in `Production`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-416">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="d97f5-417">Salvare il file ed eseguire l'app con `dotnet run` in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="d97f5-417">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="d97f5-418">Impostazioni in *appSettings. Development. JSON* non sostituisce più le impostazioni in *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-418">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="d97f5-419">Il livello di registrazione per la chiave `Logging:LogLevel:Default` è `Information`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-419">The log level for the key `Logging:LogLevel:Default` is `Information`.</span></span>

### <a name="xml-configuration-provider"></a><span data-ttu-id="d97f5-420">Provider di configurazione XML</span><span class="sxs-lookup"><span data-stu-id="d97f5-420">XML Configuration Provider</span></span>

<span data-ttu-id="d97f5-421">Il <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> carica la configurazione da coppie chiave-valore di file XML in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-421">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="d97f5-422">Per attivare la configurazione di file XML, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-422">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d97f5-423">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="d97f5-423">Overloads permit specifying:</span></span>

* <span data-ttu-id="d97f5-424">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-424">Whether the file is optional.</span></span>
* <span data-ttu-id="d97f5-425">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="d97f5-425">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="d97f5-426">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="d97f5-426">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="d97f5-427">Il nodo radice del file di configurazione viene ignorato quando vengono create le coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-427">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="d97f5-428">Non specificare una definizione DTD (Document Type Definition) o uno spazio dei nomi nel file.</span><span class="sxs-lookup"><span data-stu-id="d97f5-428">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="d97f5-429">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="d97f5-429">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="d97f5-430">I file di configurazione XML possono usare nomi di elementi distinti per le sezioni ripetute:</span><span class="sxs-lookup"><span data-stu-id="d97f5-430">XML configuration files can use distinct element names for repeating sections:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section0>
    <key0>value</key0>
    <key1>value</key1>
  </section0>
  <section1>
    <key0>value</key0>
    <key1>value</key1>
  </section1>
</configuration>
```

<span data-ttu-id="d97f5-431">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="d97f5-431">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="d97f5-432">section0:key0</span><span class="sxs-lookup"><span data-stu-id="d97f5-432">section0:key0</span></span>
* <span data-ttu-id="d97f5-433">section0:key1</span><span class="sxs-lookup"><span data-stu-id="d97f5-433">section0:key1</span></span>
* <span data-ttu-id="d97f5-434">section1:key0</span><span class="sxs-lookup"><span data-stu-id="d97f5-434">section1:key0</span></span>
* <span data-ttu-id="d97f5-435">section1:key1</span><span class="sxs-lookup"><span data-stu-id="d97f5-435">section1:key1</span></span>

<span data-ttu-id="d97f5-436">La ripetizione di elementi che usano lo stesso nome di elemento funziona se si usa l'attributo `name` per distinguere gli elementi:</span><span class="sxs-lookup"><span data-stu-id="d97f5-436">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section name="section0">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
  <section name="section1">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
</configuration>
```

<span data-ttu-id="d97f5-437">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="d97f5-437">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="d97f5-438">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="d97f5-438">section:section0:key:key0</span></span>
* <span data-ttu-id="d97f5-439">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="d97f5-439">section:section0:key:key1</span></span>
* <span data-ttu-id="d97f5-440">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="d97f5-440">section:section1:key:key0</span></span>
* <span data-ttu-id="d97f5-441">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="d97f5-441">section:section1:key:key1</span></span>

<span data-ttu-id="d97f5-442">È possibile usare attributi per fornire valori:</span><span class="sxs-lookup"><span data-stu-id="d97f5-442">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="d97f5-443">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="d97f5-443">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="d97f5-444">key:attribute</span><span class="sxs-lookup"><span data-stu-id="d97f5-444">key:attribute</span></span>
* <span data-ttu-id="d97f5-445">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="d97f5-445">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="d97f5-446">Provider di configurazione KeyPerFile</span><span class="sxs-lookup"><span data-stu-id="d97f5-446">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="d97f5-447">Il <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> usa i file di una directory come coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-447">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="d97f5-448">La chiave è il nome del file.</span><span class="sxs-lookup"><span data-stu-id="d97f5-448">The key is the file name.</span></span> <span data-ttu-id="d97f5-449">Il valore contiene il contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="d97f5-449">The value contains the file's contents.</span></span> <span data-ttu-id="d97f5-450">Il provider di configurazione KeyPerFile viene usato in scenari di hosting Docker.</span><span class="sxs-lookup"><span data-stu-id="d97f5-450">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="d97f5-451">Per attivare la configurazione chiave-per-file, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-451">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="d97f5-452">Il `directoryPath` per i file deve essere un percorso assoluto.</span><span class="sxs-lookup"><span data-stu-id="d97f5-452">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="d97f5-453">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="d97f5-453">Overloads permit specifying:</span></span>

* <span data-ttu-id="d97f5-454">Un delegato `Action<KeyPerFileConfigurationSource>` che configura l'origine.</span><span class="sxs-lookup"><span data-stu-id="d97f5-454">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="d97f5-455">Se la directory è facoltativa e il percorso della directory.</span><span class="sxs-lookup"><span data-stu-id="d97f5-455">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="d97f5-456">Il doppio carattere di sottolineatura (`__`) viene usato come delimitatore per le chiavi di configurazione nei nomi dei file.</span><span class="sxs-lookup"><span data-stu-id="d97f5-456">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="d97f5-457">Ad esempio, il nome di file `Logging__LogLevel__System` produce la chiave di configurazione `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-457">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="d97f5-458">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="d97f5-458">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="d97f5-459">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="d97f5-459">Memory Configuration Provider</span></span>

<span data-ttu-id="d97f5-460">Il <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> usa una raccolta in memoria come coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-460">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="d97f5-461">Per attivare la configurazione della raccolta in memoria, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-461">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d97f5-462">Il provider di configurazione può essere inizializzato con un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-462">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="d97f5-463">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare la configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="d97f5-463">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="d97f5-464">Nell'esempio seguente viene creato un dizionario di configurazione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-464">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="d97f5-465">Il dizionario viene usato con una chiamata a `AddInMemoryCollection` per fornire la configurazione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-465">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="d97f5-466">GetValue</span><span class="sxs-lookup"><span data-stu-id="d97f5-466">GetValue</span></span>

<span data-ttu-id="d97f5-467">[ConfigurationBinder. GetValue\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) estrae un singolo valore dalla configurazione con una chiave specificata e lo converte nel tipo non di raccolta specificato.</span><span class="sxs-lookup"><span data-stu-id="d97f5-467">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="d97f5-468">Un overload accetta un valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="d97f5-468">An overload accepts a default value.</span></span>

<span data-ttu-id="d97f5-469">L'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="d97f5-469">The following example:</span></span>

* <span data-ttu-id="d97f5-470">Estrae il valore di stringa dalla configurazione con la chiave `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-470">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="d97f5-471">Se `NumberKey` non viene trovato nelle chiavi di configurazione, viene usato il valore predefinito di `99`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-471">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="d97f5-472">Assegna al valore il tipo `int`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-472">Types the value as an `int`.</span></span>
* <span data-ttu-id="d97f5-473">Memorizza il valore nella proprietà `NumberConfig` per l'uso da parte della pagina.</span><span class="sxs-lookup"><span data-stu-id="d97f5-473">Stores the value in the `NumberConfig` property for use by the page.</span></span>

```csharp
public class IndexModel : PageModel
{
    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    public int NumberConfig { get; private set; }

    public void OnGet()
    {
        NumberConfig = _config.GetValue<int>("NumberKey", 99);
    }
}
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="d97f5-474">GetSection, GetChildren ed Exists</span><span class="sxs-lookup"><span data-stu-id="d97f5-474">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="d97f5-475">Per gli esempi che seguono, prendere in considerazione il file JSON seguente.</span><span class="sxs-lookup"><span data-stu-id="d97f5-475">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="d97f5-476">Vengono trovate quattro chiavi in due sezioni, una delle quali include una coppia di sottosezioni:</span><span class="sxs-lookup"><span data-stu-id="d97f5-476">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  },
  "section2": {
    "subsection0" : {
      "key0": "value",
      "key1": "value"
    },
    "subsection1" : {
      "key0": "value",
      "key1": "value"
    }
  }
}
```

<span data-ttu-id="d97f5-477">Quando il file viene letto nella configurazione, vengono create le chiavi gerarchiche univoche seguenti per contenere i valori di configurazione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-477">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="d97f5-478">section0:key0</span><span class="sxs-lookup"><span data-stu-id="d97f5-478">section0:key0</span></span>
* <span data-ttu-id="d97f5-479">section0:key1</span><span class="sxs-lookup"><span data-stu-id="d97f5-479">section0:key1</span></span>
* <span data-ttu-id="d97f5-480">section1:key0</span><span class="sxs-lookup"><span data-stu-id="d97f5-480">section1:key0</span></span>
* <span data-ttu-id="d97f5-481">section1:key1</span><span class="sxs-lookup"><span data-stu-id="d97f5-481">section1:key1</span></span>
* <span data-ttu-id="d97f5-482">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="d97f5-482">section2:subsection0:key0</span></span>
* <span data-ttu-id="d97f5-483">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="d97f5-483">section2:subsection0:key1</span></span>
* <span data-ttu-id="d97f5-484">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="d97f5-484">section2:subsection1:key0</span></span>
* <span data-ttu-id="d97f5-485">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="d97f5-485">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="d97f5-486">GetSection</span><span class="sxs-lookup"><span data-stu-id="d97f5-486">GetSection</span></span>

<span data-ttu-id="d97f5-487">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) estrae una sottosezione della configurazione con la chiave di sottosezione specificata.</span><span class="sxs-lookup"><span data-stu-id="d97f5-487">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="d97f5-488">Per restituire una <xref:Microsoft.Extensions.Configuration.IConfigurationSection> che contiene solo le coppie chiave-valore in `section1`, chiamare `GetSection` e fornire il nome della sezione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-488">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="d97f5-489">`configSection` non ha un valore, ma solo una chiave e un percorso.</span><span class="sxs-lookup"><span data-stu-id="d97f5-489">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="d97f5-490">In modo analogo, per ottenere i valori per le chiavi in `section2:subsection0`, chiamare `GetSection` e fornire il percorso della sezione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-490">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="d97f5-491">`GetSection` non restituisce mai `null`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-491">`GetSection` never returns `null`.</span></span> <span data-ttu-id="d97f5-492">Se non viene trovata una sezione corrispondente, viene restituita una `IConfigurationSection` vuota.</span><span class="sxs-lookup"><span data-stu-id="d97f5-492">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="d97f5-493">Quando `GetSection` restituisce una sezione corrispondente, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> non viene compilata.</span><span class="sxs-lookup"><span data-stu-id="d97f5-493">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="d97f5-494">Quando la sezione esiste, vengono restituiti <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> e <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-494">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="d97f5-495">GetChildren</span><span class="sxs-lookup"><span data-stu-id="d97f5-495">GetChildren</span></span>

<span data-ttu-id="d97f5-496">Una chiamata a [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) per `section2` ottiene una `IEnumerable<IConfigurationSection>` che include:</span><span class="sxs-lookup"><span data-stu-id="d97f5-496">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="d97f5-497">Exists</span><span class="sxs-lookup"><span data-stu-id="d97f5-497">Exists</span></span>

<span data-ttu-id="d97f5-498">Usare [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) per determinare se esiste una sezione di configurazione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-498">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="d97f5-499">Considerati i dati di esempio, `sectionExists` è `false` perché non esiste una sezione `section2:subsection2` nei dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-499">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="d97f5-500">Eseguire l'associazione a una classe</span><span class="sxs-lookup"><span data-stu-id="d97f5-500">Bind to a class</span></span>

<span data-ttu-id="d97f5-501">La configurazione può essere associata alle classi che rappresentano gruppi di impostazioni correlate usando il *modello di opzioni*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-501">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="d97f5-502">Per altre informazioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-502">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="d97f5-503">I valori di configurazione vengono restituiti come stringhe, ma la chiamata di <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> consente la costruzione di oggetti [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="d97f5-503">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="d97f5-504">Il Binder associa i valori a tutte le proprietà di lettura/scrittura pubbliche del tipo fornito.</span><span class="sxs-lookup"><span data-stu-id="d97f5-504">The binder binds values to all of the public read/write properties of the type provided.</span></span> <span data-ttu-id="d97f5-505">I campi **non** sono associati.</span><span class="sxs-lookup"><span data-stu-id="d97f5-505">Fields are **not** bound.</span></span>

<span data-ttu-id="d97f5-506">L'app di esempio contiene un modello `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="d97f5-506">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="d97f5-507">La sezione `starship` del file *starship.json* crea la configurazione quando l'app di esempio usa il provider di configurazione JSON per caricare la configurazione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-507">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

<span data-ttu-id="d97f5-508">Vengono create le coppie chiave-valore della configurazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="d97f5-508">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="d97f5-509">Chiave</span><span class="sxs-lookup"><span data-stu-id="d97f5-509">Key</span></span>                   | <span data-ttu-id="d97f5-510">valore</span><span class="sxs-lookup"><span data-stu-id="d97f5-510">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="d97f5-511">starship:name</span><span class="sxs-lookup"><span data-stu-id="d97f5-511">starship:name</span></span>         | <span data-ttu-id="d97f5-512">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="d97f5-512">USS Enterprise</span></span>                                    |
| <span data-ttu-id="d97f5-513">starship:registry</span><span class="sxs-lookup"><span data-stu-id="d97f5-513">starship:registry</span></span>     | <span data-ttu-id="d97f5-514">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="d97f5-514">NCC-1701</span></span>                                          |
| <span data-ttu-id="d97f5-515">starship:class</span><span class="sxs-lookup"><span data-stu-id="d97f5-515">starship:class</span></span>        | <span data-ttu-id="d97f5-516">Constitution</span><span class="sxs-lookup"><span data-stu-id="d97f5-516">Constitution</span></span>                                      |
| <span data-ttu-id="d97f5-517">starship:length</span><span class="sxs-lookup"><span data-stu-id="d97f5-517">starship:length</span></span>       | <span data-ttu-id="d97f5-518">304.8</span><span class="sxs-lookup"><span data-stu-id="d97f5-518">304.8</span></span>                                             |
| <span data-ttu-id="d97f5-519">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="d97f5-519">starship:commissioned</span></span> | <span data-ttu-id="d97f5-520">False</span><span class="sxs-lookup"><span data-stu-id="d97f5-520">False</span></span>                                             |
| <span data-ttu-id="d97f5-521">trademark</span><span class="sxs-lookup"><span data-stu-id="d97f5-521">trademark</span></span>             | <span data-ttu-id="d97f5-522">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="d97f5-522">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="d97f5-523">L'app di esempio chiama `GetSection` con la chiave `starship`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-523">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="d97f5-524">Le coppie chiave-valore `starship` sono isolate.</span><span class="sxs-lookup"><span data-stu-id="d97f5-524">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="d97f5-525">Il metodo `Bind` viene chiamato sulla sottosezione passando un'istanza della classe `Starship`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-525">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="d97f5-526">Dopo aver associato i valori dell'istanza, l'istanza viene assegnata a una proprietà per il rendering:</span><span class="sxs-lookup"><span data-stu-id="d97f5-526">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="d97f5-527">Associazione a un oggetto grafico</span><span class="sxs-lookup"><span data-stu-id="d97f5-527">Bind to an object graph</span></span>

<span data-ttu-id="d97f5-528"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> è in grado di associare un intero oggetto grafico POCO.</span><span class="sxs-lookup"><span data-stu-id="d97f5-528"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="d97f5-529">Come per l'associazione di un oggetto semplice, vengono associate solo le proprietà di lettura/scrittura pubbliche.</span><span class="sxs-lookup"><span data-stu-id="d97f5-529">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="d97f5-530">L'esempio contiene un modello `TvShow` il cui oggetto grafico include `Metadata` e le classi `Actors` (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="d97f5-530">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="d97f5-531">L'app di esempio ha un file *tvshow.xml* che contiene i dati di configurazione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-531">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="d97f5-532">La configurazione viene associata all'intero oggetto grafico `TvShow` con il metodo `Bind`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-532">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="d97f5-533">L'istanza associata viene assegnata a una proprietà per il rendering:</span><span class="sxs-lookup"><span data-stu-id="d97f5-533">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="d97f5-534">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) associa e restituisce il tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="d97f5-534">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="d97f5-535">`Get<T>` può essere più comodo che usare `Bind`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-535">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="d97f5-536">Il codice seguente mostra come usare `Get<T>` con l'esempio precedente, che consente di assegnare l'istanza associata direttamente alla proprietà usata per il rendering:</span><span class="sxs-lookup"><span data-stu-id="d97f5-536">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="d97f5-537">Associare una matrice a una classe</span><span class="sxs-lookup"><span data-stu-id="d97f5-537">Bind an array to a class</span></span>

<span data-ttu-id="d97f5-538">*L'app di esempio dimostra i concetti spiegati in questa sezione.*</span><span class="sxs-lookup"><span data-stu-id="d97f5-538">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="d97f5-539">Il <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supporta l'associazione di matrici agli oggetti usando gli indici delle matrici nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-539">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="d97f5-540">Qualsiasi formato di matrice che espone un segmento di chiave numerico (`:0:`, `:1:`, &hellip; `:{n}:`) è in grado di associare l'array a una matrice di classi POCO.</span><span class="sxs-lookup"><span data-stu-id="d97f5-540">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="d97f5-541">L'associazione viene fornita per convenzione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-541">Binding is provided by convention.</span></span> <span data-ttu-id="d97f5-542">I provider di configurazione personalizzati non devono implementare l'associazione di matrici.</span><span class="sxs-lookup"><span data-stu-id="d97f5-542">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="d97f5-543">**Elaborazione di matrici in memoria**</span><span class="sxs-lookup"><span data-stu-id="d97f5-543">**In-memory array processing**</span></span>

<span data-ttu-id="d97f5-544">Prendere in considerazione le chiavi di configurazione e i valori indicati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="d97f5-544">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="d97f5-545">Chiave</span><span class="sxs-lookup"><span data-stu-id="d97f5-545">Key</span></span>             | <span data-ttu-id="d97f5-546">valore</span><span class="sxs-lookup"><span data-stu-id="d97f5-546">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="d97f5-547">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="d97f5-547">array:entries:0</span></span> | <span data-ttu-id="d97f5-548">value0</span><span class="sxs-lookup"><span data-stu-id="d97f5-548">value0</span></span> |
| <span data-ttu-id="d97f5-549">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="d97f5-549">array:entries:1</span></span> | <span data-ttu-id="d97f5-550">value1</span><span class="sxs-lookup"><span data-stu-id="d97f5-550">value1</span></span> |
| <span data-ttu-id="d97f5-551">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="d97f5-551">array:entries:2</span></span> | <span data-ttu-id="d97f5-552">value2</span><span class="sxs-lookup"><span data-stu-id="d97f5-552">value2</span></span> |
| <span data-ttu-id="d97f5-553">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="d97f5-553">array:entries:4</span></span> | <span data-ttu-id="d97f5-554">value4</span><span class="sxs-lookup"><span data-stu-id="d97f5-554">value4</span></span> |
| <span data-ttu-id="d97f5-555">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="d97f5-555">array:entries:5</span></span> | <span data-ttu-id="d97f5-556">value5</span><span class="sxs-lookup"><span data-stu-id="d97f5-556">value5</span></span> |

<span data-ttu-id="d97f5-557">Queste chiavi e i valori vengono caricati nell'app di esempio usando il provider di configurazione della memoria:</span><span class="sxs-lookup"><span data-stu-id="d97f5-557">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

<span data-ttu-id="d97f5-558">La matrice ignora un valore per l'indice &num;3.</span><span class="sxs-lookup"><span data-stu-id="d97f5-558">The array skips a value for index &num;3.</span></span> <span data-ttu-id="d97f5-559">Lo strumento di associazione di configurazione non è in grado di associare valori null o di creare voci null negli oggetti associati, situazione che diventerà chiara tra poco quando viene illustrato il risultato dell'associazione di questa matrice a un oggetto.</span><span class="sxs-lookup"><span data-stu-id="d97f5-559">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="d97f5-560">Nell'app di esempio, è disponibile una classe POCO per i dati di configurazione associati:</span><span class="sxs-lookup"><span data-stu-id="d97f5-560">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="d97f5-561">I dati di configurazione sono associati all'oggetto:</span><span class="sxs-lookup"><span data-stu-id="d97f5-561">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="d97f5-562">È possibile usare anche la sintassi [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*), che produce codice più compatto:</span><span class="sxs-lookup"><span data-stu-id="d97f5-562">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="d97f5-563">L'oggetto associato, un'istanza di `ArrayExample`, riceve i dati di matrice dalla configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-563">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="d97f5-564">Indice di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="d97f5-564">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="d97f5-565">`ArrayExample.Entries` Valore</span><span class="sxs-lookup"><span data-stu-id="d97f5-565">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="d97f5-566">0</span><span class="sxs-lookup"><span data-stu-id="d97f5-566">0</span></span>                            | <span data-ttu-id="d97f5-567">value0</span><span class="sxs-lookup"><span data-stu-id="d97f5-567">value0</span></span>                       |
| <span data-ttu-id="d97f5-568">1</span><span class="sxs-lookup"><span data-stu-id="d97f5-568">1</span></span>                            | <span data-ttu-id="d97f5-569">value1</span><span class="sxs-lookup"><span data-stu-id="d97f5-569">value1</span></span>                       |
| <span data-ttu-id="d97f5-570">2</span><span class="sxs-lookup"><span data-stu-id="d97f5-570">2</span></span>                            | <span data-ttu-id="d97f5-571">value2</span><span class="sxs-lookup"><span data-stu-id="d97f5-571">value2</span></span>                       |
| <span data-ttu-id="d97f5-572">3</span><span class="sxs-lookup"><span data-stu-id="d97f5-572">3</span></span>                            | <span data-ttu-id="d97f5-573">value4</span><span class="sxs-lookup"><span data-stu-id="d97f5-573">value4</span></span>                       |
| <span data-ttu-id="d97f5-574">4</span><span class="sxs-lookup"><span data-stu-id="d97f5-574">4</span></span>                            | <span data-ttu-id="d97f5-575">value5</span><span class="sxs-lookup"><span data-stu-id="d97f5-575">value5</span></span>                       |

<span data-ttu-id="d97f5-576">L'indice &num;3 nell'oggetto associato contiene i dati di configurazione per la chiave di configurazione `array:4` e il relativo valore `value4`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-576">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="d97f5-577">Quando i dati di configurazione contenenti una matrice vengono associati, gli indici di matrice nelle chiavi di configurazione vengono usati semplicemente per scorrere i dati di configurazione quando si crea l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="d97f5-577">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="d97f5-578">Un valore null non può essere mantenuto nei dati di configurazione e una voce con valore null non viene creata in un oggetto associato quando una matrice nelle chiavi di configurazione ignora uno o più indici.</span><span class="sxs-lookup"><span data-stu-id="d97f5-578">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="d97f5-579">L'elemento di configurazione mancante per l'indice &num;3 può essere fornito prima dell'associazione all'istanza `ArrayExample` da qualsiasi provider di configurazione che produce la coppia chiave-valore corretta nella configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-579">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="d97f5-580">Se il codice di esempio include un provider di configurazione JSON aggiuntivo con la coppia chiave-valore mancante, `ArrayExample.Entries` corrisponde alla matrice di configurazione completa:</span><span class="sxs-lookup"><span data-stu-id="d97f5-580">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="d97f5-581">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="d97f5-581">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="d97f5-582">In `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="d97f5-582">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="d97f5-583">La coppia chiave-valore mostrata nella tabella viene caricata nella configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-583">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="d97f5-584">Chiave</span><span class="sxs-lookup"><span data-stu-id="d97f5-584">Key</span></span>             | <span data-ttu-id="d97f5-585">valore</span><span class="sxs-lookup"><span data-stu-id="d97f5-585">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="d97f5-586">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="d97f5-586">array:entries:3</span></span> | <span data-ttu-id="d97f5-587">value3</span><span class="sxs-lookup"><span data-stu-id="d97f5-587">value3</span></span> |

<span data-ttu-id="d97f5-588">Se l'istanza della classe `ArrayExample` viene associata dopo che il provider di configurazione JSON include la voce per l'indice &num;3, la matrice `ArrayExample.Entries` include il valore.</span><span class="sxs-lookup"><span data-stu-id="d97f5-588">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="d97f5-589">Indice di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="d97f5-589">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="d97f5-590">`ArrayExample.Entries` Valore</span><span class="sxs-lookup"><span data-stu-id="d97f5-590">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="d97f5-591">0</span><span class="sxs-lookup"><span data-stu-id="d97f5-591">0</span></span>                            | <span data-ttu-id="d97f5-592">value0</span><span class="sxs-lookup"><span data-stu-id="d97f5-592">value0</span></span>                       |
| <span data-ttu-id="d97f5-593">1</span><span class="sxs-lookup"><span data-stu-id="d97f5-593">1</span></span>                            | <span data-ttu-id="d97f5-594">value1</span><span class="sxs-lookup"><span data-stu-id="d97f5-594">value1</span></span>                       |
| <span data-ttu-id="d97f5-595">2</span><span class="sxs-lookup"><span data-stu-id="d97f5-595">2</span></span>                            | <span data-ttu-id="d97f5-596">value2</span><span class="sxs-lookup"><span data-stu-id="d97f5-596">value2</span></span>                       |
| <span data-ttu-id="d97f5-597">3</span><span class="sxs-lookup"><span data-stu-id="d97f5-597">3</span></span>                            | <span data-ttu-id="d97f5-598">value3</span><span class="sxs-lookup"><span data-stu-id="d97f5-598">value3</span></span>                       |
| <span data-ttu-id="d97f5-599">4</span><span class="sxs-lookup"><span data-stu-id="d97f5-599">4</span></span>                            | <span data-ttu-id="d97f5-600">value4</span><span class="sxs-lookup"><span data-stu-id="d97f5-600">value4</span></span>                       |
| <span data-ttu-id="d97f5-601">5</span><span class="sxs-lookup"><span data-stu-id="d97f5-601">5</span></span>                            | <span data-ttu-id="d97f5-602">value5</span><span class="sxs-lookup"><span data-stu-id="d97f5-602">value5</span></span>                       |

<span data-ttu-id="d97f5-603">**Elaborazione di matrice JSON**</span><span class="sxs-lookup"><span data-stu-id="d97f5-603">**JSON array processing**</span></span>

<span data-ttu-id="d97f5-604">Se un file JSON contiene una matrice, vengono create chiavi di configurazione per gli elementi della matrice con un indice di sezione in base zero.</span><span class="sxs-lookup"><span data-stu-id="d97f5-604">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="d97f5-605">Nel file di configurazione seguente, `subsection` è una matrice:</span><span class="sxs-lookup"><span data-stu-id="d97f5-605">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="d97f5-606">Il provider di configurazione JSON legge i dati di configurazione nelle coppie chiave-valore seguenti:</span><span class="sxs-lookup"><span data-stu-id="d97f5-606">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="d97f5-607">Chiave</span><span class="sxs-lookup"><span data-stu-id="d97f5-607">Key</span></span>                     | <span data-ttu-id="d97f5-608">valore</span><span class="sxs-lookup"><span data-stu-id="d97f5-608">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="d97f5-609">json_array:key</span><span class="sxs-lookup"><span data-stu-id="d97f5-609">json_array:key</span></span>          | <span data-ttu-id="d97f5-610">valueA</span><span class="sxs-lookup"><span data-stu-id="d97f5-610">valueA</span></span> |
| <span data-ttu-id="d97f5-611">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="d97f5-611">json_array:subsection:0</span></span> | <span data-ttu-id="d97f5-612">valueB</span><span class="sxs-lookup"><span data-stu-id="d97f5-612">valueB</span></span> |
| <span data-ttu-id="d97f5-613">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="d97f5-613">json_array:subsection:1</span></span> | <span data-ttu-id="d97f5-614">valueC</span><span class="sxs-lookup"><span data-stu-id="d97f5-614">valueC</span></span> |
| <span data-ttu-id="d97f5-615">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="d97f5-615">json_array:subsection:2</span></span> | <span data-ttu-id="d97f5-616">valueD</span><span class="sxs-lookup"><span data-stu-id="d97f5-616">valueD</span></span> |

<span data-ttu-id="d97f5-617">Nell'app di esempio, la classe POCO seguente è disponibile per associare le coppie chiave-valore della configurazione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-617">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="d97f5-618">Dopo l'associazione, `JsonArrayExample.Key` contiene il valore `valueA`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-618">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="d97f5-619">I valori di sottosezione vengono archiviati nella proprietà matrice POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-619">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="d97f5-620">Indice di `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="d97f5-620">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="d97f5-621">`JsonArrayExample.Subsection` Valore</span><span class="sxs-lookup"><span data-stu-id="d97f5-621">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="d97f5-622">0</span><span class="sxs-lookup"><span data-stu-id="d97f5-622">0</span></span>                                   | <span data-ttu-id="d97f5-623">valueB</span><span class="sxs-lookup"><span data-stu-id="d97f5-623">valueB</span></span>                              |
| <span data-ttu-id="d97f5-624">1</span><span class="sxs-lookup"><span data-stu-id="d97f5-624">1</span></span>                                   | <span data-ttu-id="d97f5-625">valueC</span><span class="sxs-lookup"><span data-stu-id="d97f5-625">valueC</span></span>                              |
| <span data-ttu-id="d97f5-626">2</span><span class="sxs-lookup"><span data-stu-id="d97f5-626">2</span></span>                                   | <span data-ttu-id="d97f5-627">valueD</span><span class="sxs-lookup"><span data-stu-id="d97f5-627">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="d97f5-628">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="d97f5-628">Custom configuration provider</span></span>

<span data-ttu-id="d97f5-629">L'app di esempio dimostra come creare un provider di configurazione di base che legge le coppie chiave-valore di configurazione da un database usando [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="d97f5-629">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="d97f5-630">Il provider ha le caratteristiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="d97f5-630">The provider has the following characteristics:</span></span>

* <span data-ttu-id="d97f5-631">Il database in memoria di Entity Framework viene usato a scopo dimostrativo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-631">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="d97f5-632">Per usare un database che richiede una stringa di connessione, implementare un `ConfigurationBuilder` secondario per fornire la stringa di connessione da un altro provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-632">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="d97f5-633">Il provider legge una tabella di database in una configurazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="d97f5-633">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="d97f5-634">Il provider non esegue una query sul database per ogni chiave.</span><span class="sxs-lookup"><span data-stu-id="d97f5-634">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="d97f5-635">Il ricaricamento in caso di modifica non è implementato, quindi l'aggiornamento del database dopo l'avvio dell'app non ha alcun effetto sulla configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="d97f5-635">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="d97f5-636">Definire un'entità `EFConfigurationValue` per l'archiviazione dei valori di configurazione nel database.</span><span class="sxs-lookup"><span data-stu-id="d97f5-636">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="d97f5-637">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="d97f5-637">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="d97f5-638">Aggiungere `EFConfigurationContext` per archiviare i valori configurati e accedervi.</span><span class="sxs-lookup"><span data-stu-id="d97f5-638">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="d97f5-639">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="d97f5-639">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="d97f5-640">Creare una classe che implementi <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-640">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="d97f5-641">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="d97f5-641">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="d97f5-642">Creare il provider di configurazione personalizzato ereditando da <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-642">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="d97f5-643">Il provider di configurazione inizializza il database quando è vuoto.</span><span class="sxs-lookup"><span data-stu-id="d97f5-643">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="d97f5-644">Poiché le [chiavi di configurazione non fanno distinzione tra maiuscole](#keys)e minuscole, il dizionario utilizzato per inizializzare il database viene creato con l'operatore di confronto senza distinzione tra maiuscole e minuscole ([StringComparer. OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span><span class="sxs-lookup"><span data-stu-id="d97f5-644">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="d97f5-645">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="d97f5-645">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="d97f5-646">Un metodo di estensione `AddEFConfiguration` consente di aggiungere l'origine di configurazione a un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-646">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="d97f5-647">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="d97f5-647">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="d97f5-648">L'esempio di codice seguente mostra come usare il `EFConfigurationProvider` personalizzato in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="d97f5-648">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="d97f5-649">Accedere alla configurazione durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="d97f5-649">Access configuration during startup</span></span>

<span data-ttu-id="d97f5-650">Inserire `IConfiguration` nel costruttore `Startup` per accedere ai valori di configurazione in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-650">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d97f5-651">Per accedere alla configurazione in `Startup.Configure`, inserire `IConfiguration` direttamente nel metodo o usare l'istanza dal costruttore:</span><span class="sxs-lookup"><span data-stu-id="d97f5-651">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

```csharp
public class Startup
{
    private readonly IConfiguration _config;

    public Startup(IConfiguration config)
    {
        _config = config;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        var value = _config["key"];
    }

    public void Configure(IApplicationBuilder app, IConfiguration config)
    {
        var value = config["key"];
    }
}
```

<span data-ttu-id="d97f5-652">Per un esempio di accesso alla configurazione usando metodi di servizio di avvio, vedere [Avvio dell'applicazione: Metodi pratici](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="d97f5-652">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="d97f5-653">Accedere alla configurazione in una pagina Razor Pages o in una visualizzazione MVC</span><span class="sxs-lookup"><span data-stu-id="d97f5-653">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="d97f5-654">Per accedere alle impostazioni di configurazione in una pagina Razor Pages o in una visualizzazione MVC, aggiungere un [direttiva using](xref:mvc/views/razor#using) ([Informazioni di riferimento su C#: direttiva using](/dotnet/csharp/language-reference/keywords/using-directive)) per lo [spazio dei nomi Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) e inserire <xref:Microsoft.Extensions.Configuration.IConfiguration> nella pagina o nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-654">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="d97f5-655">In una pagina Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="d97f5-655">In a Razor Pages page:</span></span>

```cshtml
@page
@model IndexModel
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

<span data-ttu-id="d97f5-656">In una visualizzazione MVC:</span><span class="sxs-lookup"><span data-stu-id="d97f5-656">In an MVC view:</span></span>

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="d97f5-657">Aggiungere la configurazione da un assembly esterno</span><span class="sxs-lookup"><span data-stu-id="d97f5-657">Add configuration from an external assembly</span></span>

<span data-ttu-id="d97f5-658">Un'implementazione <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> consente l'aggiunta di miglioramenti a un'app all'avvio, da un assembly esterno alla classe `Startup` dell'app.</span><span class="sxs-lookup"><span data-stu-id="d97f5-658">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="d97f5-659">Per altre informazioni, vedere <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-659">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d97f5-660">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d97f5-660">Additional resources</span></span>

* <xref:fundamentals/configuration/options>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d97f5-661">La configurazione delle app in ASP.NET Core si basa su coppie chiave-valore stabilite dai *provider di configurazione*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-661">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="d97f5-662">I provider di configurazione leggono i dati di configurazione in coppie chiave-valore da un'ampia gamma di origini di configurazione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-662">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="d97f5-663">Insieme di credenziali chiave di Azure</span><span class="sxs-lookup"><span data-stu-id="d97f5-663">Azure Key Vault</span></span>
* <span data-ttu-id="d97f5-664">Configurazione app di Azure</span><span class="sxs-lookup"><span data-stu-id="d97f5-664">Azure App Configuration</span></span>
* <span data-ttu-id="d97f5-665">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="d97f5-665">Command-line arguments</span></span>
* <span data-ttu-id="d97f5-666">Provider personalizzati (installati o creati)</span><span class="sxs-lookup"><span data-stu-id="d97f5-666">Custom providers (installed or created)</span></span>
* <span data-ttu-id="d97f5-667">File della directory</span><span class="sxs-lookup"><span data-stu-id="d97f5-667">Directory files</span></span>
* <span data-ttu-id="d97f5-668">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="d97f5-668">Environment variables</span></span>
* <span data-ttu-id="d97f5-669">Oggetti .NET in memoria</span><span class="sxs-lookup"><span data-stu-id="d97f5-669">In-memory .NET objects</span></span>
* <span data-ttu-id="d97f5-670">File di impostazioni</span><span class="sxs-lookup"><span data-stu-id="d97f5-670">Settings files</span></span>

<span data-ttu-id="d97f5-671">I pacchetti di configurazione per gli scenari di provider di configurazione comuni ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) sono inclusi nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d97f5-671">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d97f5-672">Gli esempi di codice che seguono e quelli inclusi nell'app di esempio usano lo spazio dei nomi <xref:Microsoft.Extensions.Configuration>:</span><span class="sxs-lookup"><span data-stu-id="d97f5-672">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="d97f5-673">Il *modello di opzioni* è un'estensione dei concetti di configurazione descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="d97f5-673">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="d97f5-674">Le opzioni usano le classi per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="d97f5-674">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="d97f5-675">Per altre informazioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-675">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="d97f5-676">[Visualizzare o scaricare il codice di esempio](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d97f5-676">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="d97f5-677">Host e configurazione delle app</span><span class="sxs-lookup"><span data-stu-id="d97f5-677">Host versus app configuration</span></span>

<span data-ttu-id="d97f5-678">Prima che l'app venga configurata e avviata, viene configurato e avviato un *host*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-678">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="d97f5-679">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="d97f5-679">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="d97f5-680">Sia l'app che l'host vengono configurati tramite i provider di configurazione descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="d97f5-680">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="d97f5-681">Anche le coppie chiave-valore di configurazione dell'host sono incluse nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="d97f5-681">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="d97f5-682">Per altre informazioni su come vengono usati i provider di configurazione quando viene creato l'host e sugli effetti delle origini di configurazione sull'host di configurazione, vedere <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-682">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="d97f5-683">Altra configurazione</span><span class="sxs-lookup"><span data-stu-id="d97f5-683">Other configuration</span></span>

<span data-ttu-id="d97f5-684">Questo argomento riguarda solo la *configurazione dell'app*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-684">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="d97f5-685">Altri aspetti dell'esecuzione e dell'hosting di app ASP.NET Core sono configurati usando i file di configurazione non trattati in questo argomento:</span><span class="sxs-lookup"><span data-stu-id="d97f5-685">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="d97f5-686">*Launch. json*/*launchSettings. JSON* sono i file di configurazione degli strumenti per l'ambiente di sviluppo, descritti di seguito:</span><span class="sxs-lookup"><span data-stu-id="d97f5-686">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="d97f5-687">In <xref:fundamentals/environments#development>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-687">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="d97f5-688">Nella documentazione in cui vengono usati i file per configurare ASP.NET Core app per scenari di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-688">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="d97f5-689">*Web. config* è un file di configurazione del server, descritto negli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="d97f5-689">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="d97f5-690">Per ulteriori informazioni sulla migrazione della configurazione dell'app da versioni precedenti di ASP.NET, vedere <xref:migration/proper-to-2x/index#store-configurations>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-690">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="d97f5-691">Configurazione predefinita</span><span class="sxs-lookup"><span data-stu-id="d97f5-691">Default configuration</span></span>

<span data-ttu-id="d97f5-692">Le app basate sui modelli [dotnet new](/dotnet/core/tools/dotnet-new) di ASP.NET Core chiamano <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> durante la compilazione di un host.</span><span class="sxs-lookup"><span data-stu-id="d97f5-692">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="d97f5-693">`CreateDefaultBuilder` fornisce la configurazione predefinita per l'app nell'ordine seguente:</span><span class="sxs-lookup"><span data-stu-id="d97f5-693">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="d97f5-694">La configurazione seguente si applica alle app che usano l'[host Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="d97f5-694">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="d97f5-695">Per informazioni dettagliate sulla configurazione predefinita quando viene usato l'[host generico](xref:fundamentals/host/generic-host), vedere la [versione più recente di questo argomento](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="d97f5-695">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="d97f5-696">La configurazione dell'host viene specificata da:</span><span class="sxs-lookup"><span data-stu-id="d97f5-696">Host configuration is provided from:</span></span>
  * <span data-ttu-id="d97f5-697">Variabili di ambiente con prefisso `ASPNETCORE_` (ad esempio `ASPNETCORE_ENVIRONMENT`) che usano il [provider di configurazione delle variabili di ambiente](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d97f5-697">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="d97f5-698">Il prefisso (`ASPNETCORE_`) viene rimosso al caricamento delle coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-698">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="d97f5-699">Argomenti della riga di comando che usano il [provider di configurazione della riga di comando](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d97f5-699">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="d97f5-700">La configurazione dell'app viene fornita da:</span><span class="sxs-lookup"><span data-stu-id="d97f5-700">App configuration is provided from:</span></span>
  * <span data-ttu-id="d97f5-701">*appsettings.json* mediante il [provider di configurazione dei file](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d97f5-701">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="d97f5-702">*appsettings.{Ambiente}.json* mediante il [provider di configurazione dei file](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d97f5-702">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="d97f5-703">[Strumento di gestione dei segreti](xref:security/app-secrets) quando l'app viene eseguita nell'ambiente `Development` usando l'assembly di ingresso.</span><span class="sxs-lookup"><span data-stu-id="d97f5-703">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="d97f5-704">Variabili di ambiente che usano il [provider di configurazione delle variabili di ambiente](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d97f5-704">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="d97f5-705">Argomenti della riga di comando che usano il [provider di configurazione della riga di comando](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="d97f5-705">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

## <a name="security"></a><span data-ttu-id="d97f5-706">Security</span><span class="sxs-lookup"><span data-stu-id="d97f5-706">Security</span></span>

<span data-ttu-id="d97f5-707">Per proteggere i dati di configurazione sensibili, adottare le pratiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="d97f5-707">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="d97f5-708">Non archiviare mai la password o altri dati sensibili nel codice del provider di configurazione o in file di configurazione di testo normale.</span><span class="sxs-lookup"><span data-stu-id="d97f5-708">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="d97f5-709">Non usare i segreti di produzione in ambienti di sviluppo o di test.</span><span class="sxs-lookup"><span data-stu-id="d97f5-709">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="d97f5-710">Specificare i segreti all'esterno del progetto in modo che non possano essere inavvertitamente inviati a un repository del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="d97f5-710">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="d97f5-711">Per altre informazioni, vedere gli argomenti seguenti:</span><span class="sxs-lookup"><span data-stu-id="d97f5-711">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="d97f5-712"><xref:security/app-secrets> &ndash; include consigli sull'utilizzo delle variabili di ambiente per l'archiviazione di dati sensibili.</span><span class="sxs-lookup"><span data-stu-id="d97f5-712"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="d97f5-713">Secret Manager usa il provider di configurazione dei file per archiviare i segreti utente in un file JSON nel sistema locale.</span><span class="sxs-lookup"><span data-stu-id="d97f5-713">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="d97f5-714">Il provider di configurazione dei file è descritto più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="d97f5-714">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="d97f5-715">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) archivia in modo sicuro i segreti delle app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d97f5-715">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="d97f5-716">Per altre informazioni, vedere <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-716">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="d97f5-717">Dati di configurazione gerarchici</span><span class="sxs-lookup"><span data-stu-id="d97f5-717">Hierarchical configuration data</span></span>

<span data-ttu-id="d97f5-718">L'API di configurazione è in grado di mantenere i dati di configurazione gerarchici rendendoli flat grazie all'uso di un delimitatore nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-718">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="d97f5-719">Nel file JSON seguente, esistono quattro chiavi in una gerarchia strutturata di due sezioni:</span><span class="sxs-lookup"><span data-stu-id="d97f5-719">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

<span data-ttu-id="d97f5-720">Quando il file viene letto nella configurazione, vengono create chiavi univoche per mantenere la struttura di dati gerarchici originale dell'origine di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-720">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="d97f5-721">Le sezioni e le chiavi vengono rese flat usando due punti (`:`) per mantenere la struttura originale:</span><span class="sxs-lookup"><span data-stu-id="d97f5-721">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="d97f5-722">section0:key0</span><span class="sxs-lookup"><span data-stu-id="d97f5-722">section0:key0</span></span>
* <span data-ttu-id="d97f5-723">section0:key1</span><span class="sxs-lookup"><span data-stu-id="d97f5-723">section0:key1</span></span>
* <span data-ttu-id="d97f5-724">section1:key0</span><span class="sxs-lookup"><span data-stu-id="d97f5-724">section1:key0</span></span>
* <span data-ttu-id="d97f5-725">section1:key1</span><span class="sxs-lookup"><span data-stu-id="d97f5-725">section1:key1</span></span>

<span data-ttu-id="d97f5-726">I metodi <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> e <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> sono disponibili per isolare le sezioni e gli elementi figlio di una sezione nei dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-726"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="d97f5-727">Questi metodi sono descritti più avanti in [GetSection, GetChildren ed Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="d97f5-727">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="d97f5-728">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="d97f5-728">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="d97f5-729">Origini e provider</span><span class="sxs-lookup"><span data-stu-id="d97f5-729">Sources and providers</span></span>

<span data-ttu-id="d97f5-730">All'avvio dell'app, le origini di configurazione vengono lette nell'ordine con cui sono specificati i rispettivi provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-730">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="d97f5-731">I provider di configurazione che implementano il rilevamento delle modifiche sono in grado di ricaricare la configurazione quando viene modificata un'impostazione sottostante.</span><span class="sxs-lookup"><span data-stu-id="d97f5-731">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="d97f5-732">Ad esempio, il provider di configurazione dei file (descritto più avanti in questo argomento) e il [provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration) implementano il rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="d97f5-732">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="d97f5-733"><xref:Microsoft.Extensions.Configuration.IConfiguration> è disponibile nel contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) dell'app.</span><span class="sxs-lookup"><span data-stu-id="d97f5-733"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="d97f5-734"><xref:Microsoft.Extensions.Configuration.IConfiguration> possono essere inseriti in un Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> o MVC <xref:Microsoft.AspNetCore.Mvc.Controller> per ottenere la configurazione per la classe.</span><span class="sxs-lookup"><span data-stu-id="d97f5-734"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="d97f5-735">Negli esempi seguenti viene usato il campo `_config` per accedere ai valori di configurazione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-735">In the following examples, the `_config` field is used to access configuration values:</span></span>

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }
}
```

```csharp
public class HomeController : Controller
{
    private readonly IConfiguration _config;

    public HomeController(IConfiguration config)
    {
        _config = config;
    }
}
```

<span data-ttu-id="d97f5-736">I provider di configurazione non possono usare l'inserimento delle dipendenze, perché non è disponibile quando vengono configurati dall'host.</span><span class="sxs-lookup"><span data-stu-id="d97f5-736">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="d97f5-737">Chiavi</span><span class="sxs-lookup"><span data-stu-id="d97f5-737">Keys</span></span>

<span data-ttu-id="d97f5-738">Le chiavi di configurazione adottano le convenzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="d97f5-738">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="d97f5-739">Per le chiavi non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="d97f5-739">Keys are case-insensitive.</span></span> <span data-ttu-id="d97f5-740">Ad esempio, `ConnectionString` e `connectionstring` vengono considerate chiavi equivalenti.</span><span class="sxs-lookup"><span data-stu-id="d97f5-740">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="d97f5-741">Se un valore per la stessa chiave viene impostato dallo stesso provider di configurazione o da provider diversi, viene usato l'ultimo valore impostato per la chiave.</span><span class="sxs-lookup"><span data-stu-id="d97f5-741">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="d97f5-742">Chiavi gerarchiche</span><span class="sxs-lookup"><span data-stu-id="d97f5-742">Hierarchical keys</span></span>
  * <span data-ttu-id="d97f5-743">Nell'ambito dell'API di configurazione, il separatore due punti (`:`) funziona in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="d97f5-743">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="d97f5-744">Nelle variabili di ambiente, un separatore due punti potrebbe non funzionare in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="d97f5-744">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="d97f5-745">Il doppio carattere di sottolineatura (`__`) è supportato da tutte le piattaforme e viene convertito automaticamente nei due punti.</span><span class="sxs-lookup"><span data-stu-id="d97f5-745">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="d97f5-746">In Azure Key Vault, le chiavi gerarchiche usano `--` (due trattini) come separatore.</span><span class="sxs-lookup"><span data-stu-id="d97f5-746">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="d97f5-747">Scrivere il codice per sostituire i trattini con i due punti quando i segreti vengono caricati nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="d97f5-747">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="d97f5-748">Il <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supporta l'associazione di matrici agli oggetti usando gli indici delle matrici nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-748">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="d97f5-749">L'associazione di matrici è descritta nella sezione [Associare una matrice a una classe](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="d97f5-749">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="d97f5-750">Valori</span><span class="sxs-lookup"><span data-stu-id="d97f5-750">Values</span></span>

<span data-ttu-id="d97f5-751">I valori di configurazione adottano le convenzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="d97f5-751">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="d97f5-752">I valori sono stringhe.</span><span class="sxs-lookup"><span data-stu-id="d97f5-752">Values are strings.</span></span>
* <span data-ttu-id="d97f5-753">I valori null non possono essere archiviati nella configurazione o associati a oggetti.</span><span class="sxs-lookup"><span data-stu-id="d97f5-753">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="d97f5-754">Providers</span><span class="sxs-lookup"><span data-stu-id="d97f5-754">Providers</span></span>

<span data-ttu-id="d97f5-755">La tabella seguente mostra i provider di configurazione disponibili per le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d97f5-755">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="d97f5-756">Provider</span><span class="sxs-lookup"><span data-stu-id="d97f5-756">Provider</span></span> | <span data-ttu-id="d97f5-757">Fornisce la configurazione da&hellip;</span><span class="sxs-lookup"><span data-stu-id="d97f5-757">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="d97f5-758">[Provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="d97f5-758">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="d97f5-759">Insieme di credenziali chiave di Azure</span><span class="sxs-lookup"><span data-stu-id="d97f5-759">Azure Key Vault</span></span> |
| <span data-ttu-id="d97f5-760">[Provider di Configurazione app](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Documentazione di Azure)</span><span class="sxs-lookup"><span data-stu-id="d97f5-760">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="d97f5-761">Configurazione app di Azure</span><span class="sxs-lookup"><span data-stu-id="d97f5-761">Azure App Configuration</span></span> |
| [<span data-ttu-id="d97f5-762">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="d97f5-762">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="d97f5-763">Parametri della riga di comando</span><span class="sxs-lookup"><span data-stu-id="d97f5-763">Command-line parameters</span></span> |
| [<span data-ttu-id="d97f5-764">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="d97f5-764">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="d97f5-765">Origine personalizzata</span><span class="sxs-lookup"><span data-stu-id="d97f5-765">Custom source</span></span> |
| [<span data-ttu-id="d97f5-766">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="d97f5-766">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="d97f5-767">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="d97f5-767">Environment variables</span></span> |
| [<span data-ttu-id="d97f5-768">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="d97f5-768">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="d97f5-769">File (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="d97f5-769">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="d97f5-770">Provider di configurazione chiave-per-file</span><span class="sxs-lookup"><span data-stu-id="d97f5-770">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="d97f5-771">File della directory</span><span class="sxs-lookup"><span data-stu-id="d97f5-771">Directory files</span></span> |
| [<span data-ttu-id="d97f5-772">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="d97f5-772">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="d97f5-773">Raccolte in memoria</span><span class="sxs-lookup"><span data-stu-id="d97f5-773">In-memory collections</span></span> |
| <span data-ttu-id="d97f5-774">[Segreti utente (Secret Manager)](xref:security/app-secrets) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="d97f5-774">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="d97f5-775">File nella directory dei profili utente</span><span class="sxs-lookup"><span data-stu-id="d97f5-775">File in the user profile directory</span></span> |

<span data-ttu-id="d97f5-776">Le origini di configurazione vengono lette nell'ordine in cui vengono specificati i rispetti provider di configurazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="d97f5-776">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="d97f5-777">I provider di configurazione descritti in questo argomento sono descritti in ordine alfabetico, non nell'ordine in cui il codice li dispone.</span><span class="sxs-lookup"><span data-stu-id="d97f5-777">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="d97f5-778">Ordinare i provider di configurazione nel codice in base alle priorità per le origini di configurazione sottostanti richieste dall'app.</span><span class="sxs-lookup"><span data-stu-id="d97f5-778">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="d97f5-779">Una sequenza tipica di provider di configurazione è:</span><span class="sxs-lookup"><span data-stu-id="d97f5-779">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="d97f5-780">File (*appsettings.json*, *appsettings.{Environment}.json*, dove `{Environment}` è l'ambiente di hosting corrente dell'app)</span><span class="sxs-lookup"><span data-stu-id="d97f5-780">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="d97f5-781">Insieme di credenziali chiave Azure</span><span class="sxs-lookup"><span data-stu-id="d97f5-781">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="d97f5-782">[Segreti utente (Secret Manager)](xref:security/app-secrets) (solo nell'ambiente di sviluppo)</span><span class="sxs-lookup"><span data-stu-id="d97f5-782">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="d97f5-783">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="d97f5-783">Environment variables</span></span>
1. <span data-ttu-id="d97f5-784">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="d97f5-784">Command-line arguments</span></span>

<span data-ttu-id="d97f5-785">È pratica comune posizionare il provider di configurazione della riga di comando per ultimo in una serie di provider per consentire agli argomenti della riga di comando di sostituire la configurazione impostata da altri provider.</span><span class="sxs-lookup"><span data-stu-id="d97f5-785">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="d97f5-786">La sequenza di provider precedente viene utilizzata quando un nuovo generatore host viene inizializzato con `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-786">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="d97f5-787">Per altre informazioni, vedere la sezione [Configurazione predefinita](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="d97f5-787">For more information, see the [Default configuration](#default-configuration) section.</span></span>

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="d97f5-788">Configurare il generatore di host con UseConfiguration</span><span class="sxs-lookup"><span data-stu-id="d97f5-788">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="d97f5-789">Per configurare il generatore di host, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> sul generatore di host con la configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-789">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

    var config = new ConfigurationBuilder()
        .AddInMemoryCollection(dict)
        .Build();

    return WebHost.CreateDefaultBuilder(args)
        .UseConfiguration(config)
        .UseStartup<Startup>();
}
```

## <a name="configureappconfiguration"></a><span data-ttu-id="d97f5-790">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="d97f5-790">ConfigureAppConfiguration</span></span>

<span data-ttu-id="d97f5-791">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare i provider di configurazione dell'app, oltre a quelli aggiunti automaticamente da `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d97f5-791">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="d97f5-792">Sostituire la configurazione precedente con argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="d97f5-792">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="d97f5-793">Per fornire la configurazione dell'app che può essere sostituita con argomenti della riga di comando, chiamare `AddCommandLine` per ultimo:</span><span class="sxs-lookup"><span data-stu-id="d97f5-793">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="d97f5-794">Rimuovere i provider aggiunti da CreateDefaultBuilder</span><span class="sxs-lookup"><span data-stu-id="d97f5-794">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="d97f5-795">Per rimuovere i provider aggiunti da `CreateDefaultBuilder`, chiamare prima [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) in [IConfigurationBuilder. Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) :</span><span class="sxs-lookup"><span data-stu-id="d97f5-795">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="d97f5-796">Utilizzare la configurazione durante l'avvio dell'app</span><span class="sxs-lookup"><span data-stu-id="d97f5-796">Consume configuration during app startup</span></span>

<span data-ttu-id="d97f5-797">La configurazione fornita all'app in `ConfigureAppConfiguration` è disponibile durante l'avvio dell'app, incluso `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-797">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d97f5-798">Per altre informazioni, vedere la sezione [Accedere alla configurazione durante l'avvio](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="d97f5-798">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="d97f5-799">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="d97f5-799">Command-line Configuration Provider</span></span>

<span data-ttu-id="d97f5-800"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> carica la configurazione da coppie chiave-valore di argomenti della riga di comando in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-800">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="d97f5-801">Per attivare la configurazione della riga di comando, metodo di estensione <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> viene chiamato su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-801">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d97f5-802">`AddCommandLine` viene chiamato automaticamente quando viene chiamato il metodo `CreateDefaultBuilder(string [])`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-802">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="d97f5-803">Per altre informazioni, vedere la sezione [Configurazione predefinita](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="d97f5-803">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="d97f5-804">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="d97f5-804">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="d97f5-805">Una configurazione facoltativa dai file *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-805">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="d97f5-806">[Segreti utente (Secret Manager)](xref:security/app-secrets) nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-806">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="d97f5-807">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="d97f5-807">Environment variables.</span></span>

<span data-ttu-id="d97f5-808">`CreateDefaultBuilder` aggiunge il provider di configurazione della riga di comando per ultimo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-808">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="d97f5-809">Gli argomenti della riga di comando passati in fase di esecuzione sostituiscono la configurazione impostata dagli altri provider.</span><span class="sxs-lookup"><span data-stu-id="d97f5-809">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="d97f5-810">`CreateDefaultBuilder` opera durante la creazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="d97f5-810">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="d97f5-811">Pertanto, la configurazione della riga di comando attivata da `CreateDefaultBuilder` può influire sul modo in cui l'host viene configurato.</span><span class="sxs-lookup"><span data-stu-id="d97f5-811">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="d97f5-812">Per le app basate sui modelli di ASP.NET Core, `AddCommandLine` è già stato chiamato da `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-812">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="d97f5-813">Per aggiungere altri provider di configurazione e mantenere la possibilità di sostituire la configurazione di tali provider con argomenti della riga di comando, chiamare i provider aggiuntivi dell'app in `ConfigureAppConfiguration` e quindi chiamare `AddCommandLine` per ultimo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-813">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="d97f5-814">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="d97f5-814">**Example**</span></span>

<span data-ttu-id="d97f5-815">L'app di esempio consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include una chiamata a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-815">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="d97f5-816">Aprire un prompt dei comandi nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="d97f5-816">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="d97f5-817">Fornire un argomento della riga di comando per il comando `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-817">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="d97f5-818">Dopo che l'app è in esecuzione, aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-818">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="d97f5-819">Notare che l'output contiene la coppia chiave-valore per l'argomento della riga di comando di configurazione fornito per `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-819">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="d97f5-820">Argomenti</span><span class="sxs-lookup"><span data-stu-id="d97f5-820">Arguments</span></span>

<span data-ttu-id="d97f5-821">Il valore deve seguire un segno di uguale (`=`) o la chiave deve avere un prefisso (`--` o `/`) quando il valore segue uno spazio.</span><span class="sxs-lookup"><span data-stu-id="d97f5-821">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="d97f5-822">Il valore non è necessario se viene usato un segno di uguale, ad esempio `CommandLineKey=`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-822">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="d97f5-823">Prefisso chiave</span><span class="sxs-lookup"><span data-stu-id="d97f5-823">Key prefix</span></span>               | <span data-ttu-id="d97f5-824">Esempio</span><span class="sxs-lookup"><span data-stu-id="d97f5-824">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="d97f5-825">Nessun prefisso</span><span class="sxs-lookup"><span data-stu-id="d97f5-825">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="d97f5-826">Due trattini (`--`)</span><span class="sxs-lookup"><span data-stu-id="d97f5-826">Two dashes (`--`)</span></span>        | <span data-ttu-id="d97f5-827">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="d97f5-827">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="d97f5-828">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="d97f5-828">Forward slash (`/`)</span></span>      | <span data-ttu-id="d97f5-829">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="d97f5-829">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="d97f5-830">Nello stesso comando, non mischiare coppie chiave-valore di argomenti della riga di comando che usano un segno di uguale con coppie chiave-valore che usano uno spazio.</span><span class="sxs-lookup"><span data-stu-id="d97f5-830">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="d97f5-831">Comandi di esempio:</span><span class="sxs-lookup"><span data-stu-id="d97f5-831">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="d97f5-832">Mapping di sostituzione</span><span class="sxs-lookup"><span data-stu-id="d97f5-832">Switch mappings</span></span>

<span data-ttu-id="d97f5-833">I mapping di sostituzione consentono di usare la logica di sostituzione del nome della chiave.</span><span class="sxs-lookup"><span data-stu-id="d97f5-833">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="d97f5-834">Quando si compila manualmente la configurazione con una <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, fornire un dizionario di sostituzioni switch al metodo <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-834">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="d97f5-835">Quando viene utilizzato il dizionario dei mapping di sostituzione, nel dizionario viene controllata la presenza di una chiave corrispondente alla chiave fornita da un argomento della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="d97f5-835">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="d97f5-836">Se la chiave della riga di comando viene trovata nel dizionario, il valore del dizionario (la sostituzione della chiave) viene passato nuovamente per impostare la coppia chiave-valore nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="d97f5-836">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="d97f5-837">Un mapping di sostituzione è necessario per le chiavi della riga di comando con un trattino singolo (`-`) come prefisso.</span><span class="sxs-lookup"><span data-stu-id="d97f5-837">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="d97f5-838">Regole principali del dizionario dei mapping di sostituzione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-838">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="d97f5-839">Le sostituzioni devono iniziare con un trattino (`-`) o un trattino doppio (`--`).</span><span class="sxs-lookup"><span data-stu-id="d97f5-839">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="d97f5-840">Il dizionario dei mapping di sostituzione non deve contenere chiavi duplicate.</span><span class="sxs-lookup"><span data-stu-id="d97f5-840">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="d97f5-841">Creare un dizionario dei mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-841">Create a switch mappings dictionary.</span></span> <span data-ttu-id="d97f5-842">Nell'esempio seguente vengono creati due mapping di sostituzione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-842">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="d97f5-843">Quando viene creato l'host, chiamare `AddCommandLine` con il dizionario dei mapping di sostituzione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-843">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="d97f5-844">Per le app che usano i mapping di sostituzione, la chiamata a `CreateDefaultBuilder` non deve passare argomenti.</span><span class="sxs-lookup"><span data-stu-id="d97f5-844">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="d97f5-845">La chiamata `CreateDefaultBuilder` del metodo `AddCommandLine` non include sostituzioni mappate e non è possibile passare il dizionario dei mapping di sostituzione a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-845">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="d97f5-846">La soluzione consiste nel non passare gli argomenti a `CreateDefaultBuilder` consentendo invece al metodo `ConfigurationBuilder` del metodo `AddCommandLine` di elaborare entrambi gli argomenti e il dizionario dei mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-846">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="d97f5-847">Il dizionario dei mapping di sostituzione creato contiene i dati visualizzati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="d97f5-847">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="d97f5-848">Chiave</span><span class="sxs-lookup"><span data-stu-id="d97f5-848">Key</span></span>       | <span data-ttu-id="d97f5-849">valore</span><span class="sxs-lookup"><span data-stu-id="d97f5-849">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="d97f5-850">Se le chiavi con mapping di sostituzione vengono usate all'avvio dell'app, la configurazione riceve il valore di configurazione per la chiave fornita dal dizionario:</span><span class="sxs-lookup"><span data-stu-id="d97f5-850">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="d97f5-851">Dopo aver eseguito il comando precedente, la configurazione contiene i valori mostrati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="d97f5-851">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="d97f5-852">Chiave</span><span class="sxs-lookup"><span data-stu-id="d97f5-852">Key</span></span>               | <span data-ttu-id="d97f5-853">valore</span><span class="sxs-lookup"><span data-stu-id="d97f5-853">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="d97f5-854">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="d97f5-854">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="d97f5-855"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> carica la configurazione da coppie chiave-valore di variabili di ambiente in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-855">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="d97f5-856">Per attivare la configurazione delle variabili di ambiente, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-856">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="d97f5-857">[App Azure servizio](https://azure.microsoft.com/services/app-service/) consente di impostare le variabili di ambiente nel portale di Azure che possono eseguire l'override della configurazione dell'app usando il provider di configurazione delle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="d97f5-857">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="d97f5-858">Per altre informazioni, vedere [App di Azure: Eseguire l'override della configurazione delle app usando il portale di Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="d97f5-858">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="d97f5-859">`AddEnvironmentVariables` consente di caricare variabili di ambiente con prefisso `ASPNETCORE_` per la [configurazione host](#host-versus-app-configuration) quando viene inizializzato un nuovo generatore di host con l'[host Web](xref:fundamentals/host/web-host) e viene chiamato `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-859">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="d97f5-860">Per altre informazioni, vedere la sezione [Configurazione predefinita](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="d97f5-860">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="d97f5-861">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="d97f5-861">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="d97f5-862">Configurazione delle app dalle variabili di ambiente senza prefisso chiamando `AddEnvironmentVariables` senza prefisso.</span><span class="sxs-lookup"><span data-stu-id="d97f5-862">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="d97f5-863">Una configurazione facoltativa dai file *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-863">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="d97f5-864">[Segreti utente (Secret Manager)](xref:security/app-secrets) nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-864">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="d97f5-865">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="d97f5-865">Command-line arguments.</span></span>

<span data-ttu-id="d97f5-866">Il provider di configurazione delle variabili di ambiente viene chiamato dopo aver stabilito la configurazione dai segreti utente e dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-866">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="d97f5-867">La chiamata del provider in questa posizione consente alle variabili di ambiente lette in fase di esecuzione di sostituire la configurazione impostata dai segreti utente e dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-867">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="d97f5-868">Per fornire la configurazione dell'app da variabili di ambiente aggiuntive, chiamare i provider aggiuntivi dell'app in `ConfigureAppConfiguration` e chiamare `AddEnvironmentVariables` con il prefisso:</span><span class="sxs-lookup"><span data-stu-id="d97f5-868">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="d97f5-869">Chiamare `AddEnvironmentVariables` ultimo per consentire alle variabili di ambiente con il prefisso specificato di eseguire l'override dei valori di altri provider.</span><span class="sxs-lookup"><span data-stu-id="d97f5-869">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="d97f5-870">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="d97f5-870">**Example**</span></span>

<span data-ttu-id="d97f5-871">L'app di esempio consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include una chiamata a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-871">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="d97f5-872">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d97f5-872">Run the sample app.</span></span> <span data-ttu-id="d97f5-873">Aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-873">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="d97f5-874">Notare che l'output contiene la coppia chiave-valore per la variabile di ambiente `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-874">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="d97f5-875">Il valore riflette l'ambiente in cui l'app è in esecuzione, in genere `Development` durante l'esecuzione locale.</span><span class="sxs-lookup"><span data-stu-id="d97f5-875">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="d97f5-876">Per limitare l'elenco delle variabili di ambiente restituito dall'app, l'app filtra le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="d97f5-876">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="d97f5-877">Vedere il file *Pages/Index.cshtml.cs* dell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d97f5-877">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="d97f5-878">Per esporre tutte le variabili di ambiente disponibili per l'app, modificare il `FilteredConfiguration` in *pages/index. cshtml. cs* come segue:</span><span class="sxs-lookup"><span data-stu-id="d97f5-878">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="d97f5-879">Prefissi</span><span class="sxs-lookup"><span data-stu-id="d97f5-879">Prefixes</span></span>

<span data-ttu-id="d97f5-880">Le variabili di ambiente caricate nella configurazione dell'app vengono filtrate quando si specifica un prefisso per il metodo `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-880">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="d97f5-881">Ad esempio, per filtrare le variabili di ambiente in base al prefisso `CUSTOM_`, fornire il prefisso al provider di configurazione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-881">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="d97f5-882">Il prefisso viene rimosso quando vengono create le coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-882">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="d97f5-883">Quando viene creato il generatore di host, la configurazione dell'host viene fornita dalle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="d97f5-883">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="d97f5-884">Per altre informazioni sul prefisso usato per queste variabili di ambiente, vedere la sezione [Configurazione predefinita](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="d97f5-884">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="d97f5-885">**Prefissi della stringa di connessione**</span><span class="sxs-lookup"><span data-stu-id="d97f5-885">**Connection string prefixes**</span></span>

<span data-ttu-id="d97f5-886">L'API di configurazione prevede regole di elaborazione speciali per quattro variabili di ambiente di stringa di connessione coinvolte nella configurazione di stringhe di connessione di Azure per l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="d97f5-886">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="d97f5-887">Le variabili di ambiente con i prefissi indicati nella tabella vengono caricate nell'app se non viene fornito alcun prefisso a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-887">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="d97f5-888">Prefisso della stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="d97f5-888">Connection string prefix</span></span> | <span data-ttu-id="d97f5-889">Provider</span><span class="sxs-lookup"><span data-stu-id="d97f5-889">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="d97f5-890">Provider personalizzato</span><span class="sxs-lookup"><span data-stu-id="d97f5-890">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="d97f5-891">MySQL</span><span class="sxs-lookup"><span data-stu-id="d97f5-891">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="d97f5-892">Database SQL di Azure</span><span class="sxs-lookup"><span data-stu-id="d97f5-892">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="d97f5-893">SQL Server</span><span class="sxs-lookup"><span data-stu-id="d97f5-893">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="d97f5-894">Quando una variabile di ambiente viene individuata e caricata nella configurazione con uno qualsiasi dei quattro prefissi indicati nella tabella:</span><span class="sxs-lookup"><span data-stu-id="d97f5-894">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="d97f5-895">La chiave di configurazione viene creata rimuovendo il prefisso della variabile di ambiente e aggiungendo una sezione per la chiave di configurazione (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="d97f5-895">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="d97f5-896">Viene creata una nuova coppia chiave-valore della configurazione che rappresenta il provider di connessione al database (ad eccezione di `CUSTOMCONNSTR_`, che non ha un provider dichiarato).</span><span class="sxs-lookup"><span data-stu-id="d97f5-896">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="d97f5-897">Chiave della variabile di ambiente</span><span class="sxs-lookup"><span data-stu-id="d97f5-897">Environment variable key</span></span> | <span data-ttu-id="d97f5-898">Chiave di configurazione convertita</span><span class="sxs-lookup"><span data-stu-id="d97f5-898">Converted configuration key</span></span> | <span data-ttu-id="d97f5-899">Voce di configurazione del provider</span><span class="sxs-lookup"><span data-stu-id="d97f5-899">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="d97f5-900">Voce di configurazione non creata.</span><span class="sxs-lookup"><span data-stu-id="d97f5-900">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="d97f5-901">Chiave: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="d97f5-901">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="d97f5-902">Valore: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="d97f5-902">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="d97f5-903">Chiave: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="d97f5-903">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="d97f5-904">Valore: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="d97f5-904">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="d97f5-905">Chiave: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="d97f5-905">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="d97f5-906">Valore: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="d97f5-906">Value: `System.Data.SqlClient`</span></span>  |

<span data-ttu-id="d97f5-907">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="d97f5-907">**Example**</span></span>

<span data-ttu-id="d97f5-908">Nel server viene creata una variabile di ambiente della stringa di connessione personalizzata:</span><span class="sxs-lookup"><span data-stu-id="d97f5-908">A custom connection string environment variable is created on the server:</span></span>

* <span data-ttu-id="d97f5-909">Nome &ndash; `CUSTOMCONNSTR_ReleaseDB`</span><span class="sxs-lookup"><span data-stu-id="d97f5-909">Name &ndash; `CUSTOMCONNSTR_ReleaseDB`</span></span>
* <span data-ttu-id="d97f5-910">Valore &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span><span class="sxs-lookup"><span data-stu-id="d97f5-910">Value &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span></span>

<span data-ttu-id="d97f5-911">Se `IConfiguration` viene inserito e assegnato a un campo denominato `_config`, leggere il valore:</span><span class="sxs-lookup"><span data-stu-id="d97f5-911">If `IConfiguration` is injected and assigned to a field named `_config`, read the value:</span></span>

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a><span data-ttu-id="d97f5-912">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="d97f5-912">File Configuration Provider</span></span>

<span data-ttu-id="d97f5-913"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> è la classe base per il caricamento della configurazione dal file system.</span><span class="sxs-lookup"><span data-stu-id="d97f5-913"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="d97f5-914">I provider di configurazione seguenti sono dedicati per specifici tipi di file:</span><span class="sxs-lookup"><span data-stu-id="d97f5-914">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="d97f5-915">Provider di configurazione INI</span><span class="sxs-lookup"><span data-stu-id="d97f5-915">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="d97f5-916">Provider di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="d97f5-916">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="d97f5-917">Provider di configurazione XML</span><span class="sxs-lookup"><span data-stu-id="d97f5-917">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="d97f5-918">Provider di configurazione INI</span><span class="sxs-lookup"><span data-stu-id="d97f5-918">INI Configuration Provider</span></span>

<span data-ttu-id="d97f5-919"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> carica la configurazione da coppie chiave-valore di file INI in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-919">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="d97f5-920">Per attivare la configurazione di file INI, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-920">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d97f5-921">I due punti possono essere usati come delimitatore di sezione nella configurazione di file INI.</span><span class="sxs-lookup"><span data-stu-id="d97f5-921">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="d97f5-922">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="d97f5-922">Overloads permit specifying:</span></span>

* <span data-ttu-id="d97f5-923">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-923">Whether the file is optional.</span></span>
* <span data-ttu-id="d97f5-924">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="d97f5-924">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="d97f5-925">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="d97f5-925">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="d97f5-926">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="d97f5-926">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="d97f5-927">Un esempio generico di un file di configurazione INI:</span><span class="sxs-lookup"><span data-stu-id="d97f5-927">A generic example of an INI configuration file:</span></span>

```ini
[section0]
key0=value
key1=value

[section1]
subsection:key=value

[section2:subsection0]
key=value

[section2:subsection1]
key=value
```

<span data-ttu-id="d97f5-928">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="d97f5-928">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="d97f5-929">section0:key0</span><span class="sxs-lookup"><span data-stu-id="d97f5-929">section0:key0</span></span>
* <span data-ttu-id="d97f5-930">section0:key1</span><span class="sxs-lookup"><span data-stu-id="d97f5-930">section0:key1</span></span>
* <span data-ttu-id="d97f5-931">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="d97f5-931">section1:subsection:key</span></span>
* <span data-ttu-id="d97f5-932">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="d97f5-932">section2:subsection0:key</span></span>
* <span data-ttu-id="d97f5-933">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="d97f5-933">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="d97f5-934">Provider di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="d97f5-934">JSON Configuration Provider</span></span>

<span data-ttu-id="d97f5-935">Il <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> carica la configurazione da coppie chiave-valore di file JSON in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-935">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="d97f5-936">Per attivare la configurazione di file JSON, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-936">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d97f5-937">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="d97f5-937">Overloads permit specifying:</span></span>

* <span data-ttu-id="d97f5-938">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-938">Whether the file is optional.</span></span>
* <span data-ttu-id="d97f5-939">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="d97f5-939">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="d97f5-940">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="d97f5-940">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="d97f5-941">`AddJsonFile` viene chiamato automaticamente due volte quando un nuovo generatore host viene inizializzato con `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-941">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="d97f5-942">Il metodo viene chiamato per caricare la configurazione da:</span><span class="sxs-lookup"><span data-stu-id="d97f5-942">The method is called to load configuration from:</span></span>

* <span data-ttu-id="d97f5-943">*appSettings. json* &ndash; questo file viene letto per primo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-943">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="d97f5-944">La versione dell'ambiente del file può sostituire i valori forniti dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-944">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="d97f5-945">*appSettings. {Environment}. JSON* &ndash; la versione dell'ambiente del file viene caricata in base a [IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="d97f5-945">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="d97f5-946">Per altre informazioni, vedere la sezione [Configurazione predefinita](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="d97f5-946">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="d97f5-947">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="d97f5-947">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="d97f5-948">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="d97f5-948">Environment variables.</span></span>
* <span data-ttu-id="d97f5-949">[Segreti utente (Secret Manager)](xref:security/app-secrets) nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-949">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="d97f5-950">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="d97f5-950">Command-line arguments.</span></span>

<span data-ttu-id="d97f5-951">Il provider di configurazione JSON viene stabilito per primo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-951">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="d97f5-952">I segreti utente, le variabili di ambiente e gli argomenti della riga di comando sostituiscono quindi la configurazione impostata dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-952">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="d97f5-953">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare la configurazione dell'app per file diversi da *appsettings.json* e *appsettings.{Environment}.json*:</span><span class="sxs-lookup"><span data-stu-id="d97f5-953">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="d97f5-954">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="d97f5-954">**Example**</span></span>

<span data-ttu-id="d97f5-955">L'app di esempio sfrutta il metodo di convenienza statica `CreateDefaultBuilder` per compilare l'host, che include due chiamate al `AddJsonFile`:</span><span class="sxs-lookup"><span data-stu-id="d97f5-955">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

* <span data-ttu-id="d97f5-956">La prima chiamata a `AddJsonFile` carica la configurazione da *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="d97f5-956">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="d97f5-957">La seconda chiamata a `AddJsonFile` carica la configurazione da *appSettings. { Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-957">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="d97f5-958">Per *appSettings. Development. JSON* nell'app di esempio, viene caricato il file seguente:</span><span class="sxs-lookup"><span data-stu-id="d97f5-958">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="d97f5-959">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="d97f5-959">Run the sample app.</span></span> <span data-ttu-id="d97f5-960">Aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-960">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="d97f5-961">L'output contiene coppie chiave-valore per la configurazione basata sull'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="d97f5-961">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="d97f5-962">Il livello di registrazione per la chiave `Logging:LogLevel:Default` viene `Debug` quando si esegue l'app nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-962">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="d97f5-963">Eseguire di nuovo l'app di esempio nell'ambiente di produzione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-963">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="d97f5-964">Aprire il file *Properties/launchSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="d97f5-964">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="d97f5-965">Nel profilo di `ConfigurationSample` modificare il valore della variabile di ambiente `ASPNETCORE_ENVIRONMENT` in `Production`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-965">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="d97f5-966">Salvare il file ed eseguire l'app con `dotnet run` in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="d97f5-966">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="d97f5-967">Impostazioni in *appSettings. Development. JSON* non sostituisce più le impostazioni in *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-967">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="d97f5-968">Il livello di registrazione per la chiave `Logging:LogLevel:Default` è `Warning`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-968">The log level for the key `Logging:LogLevel:Default` is `Warning`.</span></span>

### <a name="xml-configuration-provider"></a><span data-ttu-id="d97f5-969">Provider di configurazione XML</span><span class="sxs-lookup"><span data-stu-id="d97f5-969">XML Configuration Provider</span></span>

<span data-ttu-id="d97f5-970">Il <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> carica la configurazione da coppie chiave-valore di file XML in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-970">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="d97f5-971">Per attivare la configurazione di file XML, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-971">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d97f5-972">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="d97f5-972">Overloads permit specifying:</span></span>

* <span data-ttu-id="d97f5-973">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-973">Whether the file is optional.</span></span>
* <span data-ttu-id="d97f5-974">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="d97f5-974">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="d97f5-975">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="d97f5-975">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="d97f5-976">Il nodo radice del file di configurazione viene ignorato quando vengono create le coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-976">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="d97f5-977">Non specificare una definizione DTD (Document Type Definition) o uno spazio dei nomi nel file.</span><span class="sxs-lookup"><span data-stu-id="d97f5-977">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="d97f5-978">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="d97f5-978">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="d97f5-979">I file di configurazione XML possono usare nomi di elementi distinti per le sezioni ripetute:</span><span class="sxs-lookup"><span data-stu-id="d97f5-979">XML configuration files can use distinct element names for repeating sections:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section0>
    <key0>value</key0>
    <key1>value</key1>
  </section0>
  <section1>
    <key0>value</key0>
    <key1>value</key1>
  </section1>
</configuration>
```

<span data-ttu-id="d97f5-980">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="d97f5-980">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="d97f5-981">section0:key0</span><span class="sxs-lookup"><span data-stu-id="d97f5-981">section0:key0</span></span>
* <span data-ttu-id="d97f5-982">section0:key1</span><span class="sxs-lookup"><span data-stu-id="d97f5-982">section0:key1</span></span>
* <span data-ttu-id="d97f5-983">section1:key0</span><span class="sxs-lookup"><span data-stu-id="d97f5-983">section1:key0</span></span>
* <span data-ttu-id="d97f5-984">section1:key1</span><span class="sxs-lookup"><span data-stu-id="d97f5-984">section1:key1</span></span>

<span data-ttu-id="d97f5-985">La ripetizione di elementi che usano lo stesso nome di elemento funziona se si usa l'attributo `name` per distinguere gli elementi:</span><span class="sxs-lookup"><span data-stu-id="d97f5-985">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section name="section0">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
  <section name="section1">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
</configuration>
```

<span data-ttu-id="d97f5-986">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="d97f5-986">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="d97f5-987">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="d97f5-987">section:section0:key:key0</span></span>
* <span data-ttu-id="d97f5-988">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="d97f5-988">section:section0:key:key1</span></span>
* <span data-ttu-id="d97f5-989">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="d97f5-989">section:section1:key:key0</span></span>
* <span data-ttu-id="d97f5-990">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="d97f5-990">section:section1:key:key1</span></span>

<span data-ttu-id="d97f5-991">È possibile usare attributi per fornire valori:</span><span class="sxs-lookup"><span data-stu-id="d97f5-991">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="d97f5-992">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="d97f5-992">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="d97f5-993">key:attribute</span><span class="sxs-lookup"><span data-stu-id="d97f5-993">key:attribute</span></span>
* <span data-ttu-id="d97f5-994">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="d97f5-994">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="d97f5-995">Provider di configurazione KeyPerFile</span><span class="sxs-lookup"><span data-stu-id="d97f5-995">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="d97f5-996">Il <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> usa i file di una directory come coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-996">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="d97f5-997">La chiave è il nome del file.</span><span class="sxs-lookup"><span data-stu-id="d97f5-997">The key is the file name.</span></span> <span data-ttu-id="d97f5-998">Il valore contiene il contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="d97f5-998">The value contains the file's contents.</span></span> <span data-ttu-id="d97f5-999">Il provider di configurazione KeyPerFile viene usato in scenari di hosting Docker.</span><span class="sxs-lookup"><span data-stu-id="d97f5-999">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="d97f5-1000">Per attivare la configurazione chiave-per-file, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1000">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="d97f5-1001">Il `directoryPath` per i file deve essere un percorso assoluto.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1001">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="d97f5-1002">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1002">Overloads permit specifying:</span></span>

* <span data-ttu-id="d97f5-1003">Un delegato `Action<KeyPerFileConfigurationSource>` che configura l'origine.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1003">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="d97f5-1004">Se la directory è facoltativa e il percorso della directory.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1004">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="d97f5-1005">Il doppio carattere di sottolineatura (`__`) viene usato come delimitatore per le chiavi di configurazione nei nomi dei file.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1005">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="d97f5-1006">Ad esempio, il nome di file `Logging__LogLevel__System` produce la chiave di configurazione `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1006">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="d97f5-1007">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1007">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="d97f5-1008">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="d97f5-1008">Memory Configuration Provider</span></span>

<span data-ttu-id="d97f5-1009">Il <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> usa una raccolta in memoria come coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1009">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="d97f5-1010">Per attivare la configurazione della raccolta in memoria, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1010">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="d97f5-1011">Il provider di configurazione può essere inizializzato con un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1011">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="d97f5-1012">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare la configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1012">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="d97f5-1013">Nell'esempio seguente viene creato un dizionario di configurazione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1013">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="d97f5-1014">Il dizionario viene usato con una chiamata a `AddInMemoryCollection` per fornire la configurazione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1014">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="d97f5-1015">GetValue</span><span class="sxs-lookup"><span data-stu-id="d97f5-1015">GetValue</span></span>

<span data-ttu-id="d97f5-1016">[ConfigurationBinder. GetValue\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) estrae un singolo valore dalla configurazione con una chiave specificata e lo converte nel tipo non di raccolta specificato.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1016">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="d97f5-1017">Un overload accetta un valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1017">An overload accepts a default value.</span></span>

<span data-ttu-id="d97f5-1018">L'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1018">The following example:</span></span>

* <span data-ttu-id="d97f5-1019">Estrae il valore di stringa dalla configurazione con la chiave `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1019">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="d97f5-1020">Se `NumberKey` non viene trovato nelle chiavi di configurazione, viene usato il valore predefinito di `99`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1020">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="d97f5-1021">Assegna al valore il tipo `int`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1021">Types the value as an `int`.</span></span>
* <span data-ttu-id="d97f5-1022">Memorizza il valore nella proprietà `NumberConfig` per l'uso da parte della pagina.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1022">Stores the value in the `NumberConfig` property for use by the page.</span></span>

```csharp
public class IndexModel : PageModel
{
    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    public int NumberConfig { get; private set; }

    public void OnGet()
    {
        NumberConfig = _config.GetValue<int>("NumberKey", 99);
    }
}
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="d97f5-1023">GetSection, GetChildren ed Exists</span><span class="sxs-lookup"><span data-stu-id="d97f5-1023">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="d97f5-1024">Per gli esempi che seguono, prendere in considerazione il file JSON seguente.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1024">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="d97f5-1025">Vengono trovate quattro chiavi in due sezioni, una delle quali include una coppia di sottosezioni:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1025">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  },
  "section2": {
    "subsection0" : {
      "key0": "value",
      "key1": "value"
    },
    "subsection1" : {
      "key0": "value",
      "key1": "value"
    }
  }
}
```

<span data-ttu-id="d97f5-1026">Quando il file viene letto nella configurazione, vengono create le chiavi gerarchiche univoche seguenti per contenere i valori di configurazione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1026">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="d97f5-1027">section0:key0</span><span class="sxs-lookup"><span data-stu-id="d97f5-1027">section0:key0</span></span>
* <span data-ttu-id="d97f5-1028">section0:key1</span><span class="sxs-lookup"><span data-stu-id="d97f5-1028">section0:key1</span></span>
* <span data-ttu-id="d97f5-1029">section1:key0</span><span class="sxs-lookup"><span data-stu-id="d97f5-1029">section1:key0</span></span>
* <span data-ttu-id="d97f5-1030">section1:key1</span><span class="sxs-lookup"><span data-stu-id="d97f5-1030">section1:key1</span></span>
* <span data-ttu-id="d97f5-1031">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="d97f5-1031">section2:subsection0:key0</span></span>
* <span data-ttu-id="d97f5-1032">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="d97f5-1032">section2:subsection0:key1</span></span>
* <span data-ttu-id="d97f5-1033">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="d97f5-1033">section2:subsection1:key0</span></span>
* <span data-ttu-id="d97f5-1034">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="d97f5-1034">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="d97f5-1035">GetSection</span><span class="sxs-lookup"><span data-stu-id="d97f5-1035">GetSection</span></span>

<span data-ttu-id="d97f5-1036">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) estrae una sottosezione della configurazione con la chiave di sottosezione specificata.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1036">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="d97f5-1037">Per restituire una <xref:Microsoft.Extensions.Configuration.IConfigurationSection> che contiene solo le coppie chiave-valore in `section1`, chiamare `GetSection` e fornire il nome della sezione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1037">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="d97f5-1038">`configSection` non ha un valore, ma solo una chiave e un percorso.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1038">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="d97f5-1039">In modo analogo, per ottenere i valori per le chiavi in `section2:subsection0`, chiamare `GetSection` e fornire il percorso della sezione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1039">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="d97f5-1040">`GetSection` non restituisce mai `null`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1040">`GetSection` never returns `null`.</span></span> <span data-ttu-id="d97f5-1041">Se non viene trovata una sezione corrispondente, viene restituita una `IConfigurationSection` vuota.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1041">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="d97f5-1042">Quando `GetSection` restituisce una sezione corrispondente, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> non viene compilata.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1042">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="d97f5-1043">Quando la sezione esiste, vengono restituiti <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> e <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1043">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="d97f5-1044">GetChildren</span><span class="sxs-lookup"><span data-stu-id="d97f5-1044">GetChildren</span></span>

<span data-ttu-id="d97f5-1045">Una chiamata a [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) per `section2` ottiene una `IEnumerable<IConfigurationSection>` che include:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1045">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="d97f5-1046">Exists</span><span class="sxs-lookup"><span data-stu-id="d97f5-1046">Exists</span></span>

<span data-ttu-id="d97f5-1047">Usare [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) per determinare se esiste una sezione di configurazione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1047">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="d97f5-1048">Considerati i dati di esempio, `sectionExists` è `false` perché non esiste una sezione `section2:subsection2` nei dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1048">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="d97f5-1049">Eseguire l'associazione a una classe</span><span class="sxs-lookup"><span data-stu-id="d97f5-1049">Bind to a class</span></span>

<span data-ttu-id="d97f5-1050">La configurazione può essere associata alle classi che rappresentano gruppi di impostazioni correlate usando il *modello di opzioni*.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1050">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="d97f5-1051">Per altre informazioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1051">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="d97f5-1052">I valori di configurazione vengono restituiti come stringhe, ma la chiamata di <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> consente la costruzione di oggetti [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="d97f5-1052">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="d97f5-1053">Il Binder associa i valori a tutte le proprietà di lettura/scrittura pubbliche del tipo fornito.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1053">The binder binds values to all of the public read/write properties of the type provided.</span></span> <span data-ttu-id="d97f5-1054">I campi **non** sono associati.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1054">Fields are **not** bound.</span></span>

<span data-ttu-id="d97f5-1055">L'app di esempio contiene un modello `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="d97f5-1055">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="d97f5-1056">La sezione `starship` del file *starship.json* crea la configurazione quando l'app di esempio usa il provider di configurazione JSON per caricare la configurazione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1056">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

<span data-ttu-id="d97f5-1057">Vengono create le coppie chiave-valore della configurazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1057">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="d97f5-1058">Chiave</span><span class="sxs-lookup"><span data-stu-id="d97f5-1058">Key</span></span>                   | <span data-ttu-id="d97f5-1059">valore</span><span class="sxs-lookup"><span data-stu-id="d97f5-1059">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="d97f5-1060">starship:name</span><span class="sxs-lookup"><span data-stu-id="d97f5-1060">starship:name</span></span>         | <span data-ttu-id="d97f5-1061">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="d97f5-1061">USS Enterprise</span></span>                                    |
| <span data-ttu-id="d97f5-1062">starship:registry</span><span class="sxs-lookup"><span data-stu-id="d97f5-1062">starship:registry</span></span>     | <span data-ttu-id="d97f5-1063">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="d97f5-1063">NCC-1701</span></span>                                          |
| <span data-ttu-id="d97f5-1064">starship:class</span><span class="sxs-lookup"><span data-stu-id="d97f5-1064">starship:class</span></span>        | <span data-ttu-id="d97f5-1065">Constitution</span><span class="sxs-lookup"><span data-stu-id="d97f5-1065">Constitution</span></span>                                      |
| <span data-ttu-id="d97f5-1066">starship:length</span><span class="sxs-lookup"><span data-stu-id="d97f5-1066">starship:length</span></span>       | <span data-ttu-id="d97f5-1067">304.8</span><span class="sxs-lookup"><span data-stu-id="d97f5-1067">304.8</span></span>                                             |
| <span data-ttu-id="d97f5-1068">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="d97f5-1068">starship:commissioned</span></span> | <span data-ttu-id="d97f5-1069">False</span><span class="sxs-lookup"><span data-stu-id="d97f5-1069">False</span></span>                                             |
| <span data-ttu-id="d97f5-1070">trademark</span><span class="sxs-lookup"><span data-stu-id="d97f5-1070">trademark</span></span>             | <span data-ttu-id="d97f5-1071">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="d97f5-1071">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="d97f5-1072">L'app di esempio chiama `GetSection` con la chiave `starship`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1072">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="d97f5-1073">Le coppie chiave-valore `starship` sono isolate.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1073">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="d97f5-1074">Il metodo `Bind` viene chiamato sulla sottosezione passando un'istanza della classe `Starship`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1074">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="d97f5-1075">Dopo aver associato i valori dell'istanza, l'istanza viene assegnata a una proprietà per il rendering:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1075">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="d97f5-1076">Associazione a un oggetto grafico</span><span class="sxs-lookup"><span data-stu-id="d97f5-1076">Bind to an object graph</span></span>

<span data-ttu-id="d97f5-1077"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> è in grado di associare un intero oggetto grafico POCO.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1077"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="d97f5-1078">Come per l'associazione di un oggetto semplice, vengono associate solo le proprietà di lettura/scrittura pubbliche.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1078">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="d97f5-1079">L'esempio contiene un modello `TvShow` il cui oggetto grafico include `Metadata` e le classi `Actors` (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="d97f5-1079">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="d97f5-1080">L'app di esempio ha un file *tvshow.xml* che contiene i dati di configurazione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1080">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="d97f5-1081">La configurazione viene associata all'intero oggetto grafico `TvShow` con il metodo `Bind`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1081">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="d97f5-1082">L'istanza associata viene assegnata a una proprietà per il rendering:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1082">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="d97f5-1083">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) associa e restituisce il tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1083">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="d97f5-1084">`Get<T>` può essere più comodo che usare `Bind`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1084">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="d97f5-1085">Il codice seguente mostra come usare `Get<T>` con l'esempio precedente, che consente di assegnare l'istanza associata direttamente alla proprietà usata per il rendering:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1085">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="d97f5-1086">Associare una matrice a una classe</span><span class="sxs-lookup"><span data-stu-id="d97f5-1086">Bind an array to a class</span></span>

<span data-ttu-id="d97f5-1087">*L'app di esempio dimostra i concetti spiegati in questa sezione.*</span><span class="sxs-lookup"><span data-stu-id="d97f5-1087">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="d97f5-1088">Il <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supporta l'associazione di matrici agli oggetti usando gli indici delle matrici nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1088">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="d97f5-1089">Qualsiasi formato di matrice che espone un segmento di chiave numerico (`:0:`, `:1:`, &hellip; `:{n}:`) è in grado di associare l'array a una matrice di classi POCO.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1089">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="d97f5-1090">L'associazione viene fornita per convenzione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1090">Binding is provided by convention.</span></span> <span data-ttu-id="d97f5-1091">I provider di configurazione personalizzati non devono implementare l'associazione di matrici.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1091">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="d97f5-1092">**Elaborazione di matrici in memoria**</span><span class="sxs-lookup"><span data-stu-id="d97f5-1092">**In-memory array processing**</span></span>

<span data-ttu-id="d97f5-1093">Prendere in considerazione le chiavi di configurazione e i valori indicati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1093">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="d97f5-1094">Chiave</span><span class="sxs-lookup"><span data-stu-id="d97f5-1094">Key</span></span>             | <span data-ttu-id="d97f5-1095">valore</span><span class="sxs-lookup"><span data-stu-id="d97f5-1095">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="d97f5-1096">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="d97f5-1096">array:entries:0</span></span> | <span data-ttu-id="d97f5-1097">value0</span><span class="sxs-lookup"><span data-stu-id="d97f5-1097">value0</span></span> |
| <span data-ttu-id="d97f5-1098">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="d97f5-1098">array:entries:1</span></span> | <span data-ttu-id="d97f5-1099">value1</span><span class="sxs-lookup"><span data-stu-id="d97f5-1099">value1</span></span> |
| <span data-ttu-id="d97f5-1100">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="d97f5-1100">array:entries:2</span></span> | <span data-ttu-id="d97f5-1101">value2</span><span class="sxs-lookup"><span data-stu-id="d97f5-1101">value2</span></span> |
| <span data-ttu-id="d97f5-1102">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="d97f5-1102">array:entries:4</span></span> | <span data-ttu-id="d97f5-1103">value4</span><span class="sxs-lookup"><span data-stu-id="d97f5-1103">value4</span></span> |
| <span data-ttu-id="d97f5-1104">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="d97f5-1104">array:entries:5</span></span> | <span data-ttu-id="d97f5-1105">value5</span><span class="sxs-lookup"><span data-stu-id="d97f5-1105">value5</span></span> |

<span data-ttu-id="d97f5-1106">Queste chiavi e i valori vengono caricati nell'app di esempio usando il provider di configurazione della memoria:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1106">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

<span data-ttu-id="d97f5-1107">La matrice ignora un valore per l'indice &num;3.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1107">The array skips a value for index &num;3.</span></span> <span data-ttu-id="d97f5-1108">Lo strumento di associazione di configurazione non è in grado di associare valori null o di creare voci null negli oggetti associati, situazione che diventerà chiara tra poco quando viene illustrato il risultato dell'associazione di questa matrice a un oggetto.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1108">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="d97f5-1109">Nell'app di esempio, è disponibile una classe POCO per i dati di configurazione associati:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1109">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="d97f5-1110">I dati di configurazione sono associati all'oggetto:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1110">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="d97f5-1111">È possibile usare anche la sintassi [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*), che produce codice più compatto:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1111">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="d97f5-1112">L'oggetto associato, un'istanza di `ArrayExample`, riceve i dati di matrice dalla configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1112">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="d97f5-1113">Indice di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="d97f5-1113">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="d97f5-1114">`ArrayExample.Entries` Valore</span><span class="sxs-lookup"><span data-stu-id="d97f5-1114">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="d97f5-1115">0</span><span class="sxs-lookup"><span data-stu-id="d97f5-1115">0</span></span>                            | <span data-ttu-id="d97f5-1116">value0</span><span class="sxs-lookup"><span data-stu-id="d97f5-1116">value0</span></span>                       |
| <span data-ttu-id="d97f5-1117">1</span><span class="sxs-lookup"><span data-stu-id="d97f5-1117">1</span></span>                            | <span data-ttu-id="d97f5-1118">value1</span><span class="sxs-lookup"><span data-stu-id="d97f5-1118">value1</span></span>                       |
| <span data-ttu-id="d97f5-1119">2</span><span class="sxs-lookup"><span data-stu-id="d97f5-1119">2</span></span>                            | <span data-ttu-id="d97f5-1120">value2</span><span class="sxs-lookup"><span data-stu-id="d97f5-1120">value2</span></span>                       |
| <span data-ttu-id="d97f5-1121">3</span><span class="sxs-lookup"><span data-stu-id="d97f5-1121">3</span></span>                            | <span data-ttu-id="d97f5-1122">value4</span><span class="sxs-lookup"><span data-stu-id="d97f5-1122">value4</span></span>                       |
| <span data-ttu-id="d97f5-1123">4</span><span class="sxs-lookup"><span data-stu-id="d97f5-1123">4</span></span>                            | <span data-ttu-id="d97f5-1124">value5</span><span class="sxs-lookup"><span data-stu-id="d97f5-1124">value5</span></span>                       |

<span data-ttu-id="d97f5-1125">L'indice &num;3 nell'oggetto associato contiene i dati di configurazione per la chiave di configurazione `array:4` e il relativo valore `value4`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1125">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="d97f5-1126">Quando i dati di configurazione contenenti una matrice vengono associati, gli indici di matrice nelle chiavi di configurazione vengono usati semplicemente per scorrere i dati di configurazione quando si crea l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1126">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="d97f5-1127">Un valore null non può essere mantenuto nei dati di configurazione e una voce con valore null non viene creata in un oggetto associato quando una matrice nelle chiavi di configurazione ignora uno o più indici.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1127">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="d97f5-1128">L'elemento di configurazione mancante per l'indice &num;3 può essere fornito prima dell'associazione all'istanza `ArrayExample` da qualsiasi provider di configurazione che produce la coppia chiave-valore corretta nella configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1128">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="d97f5-1129">Se il codice di esempio include un provider di configurazione JSON aggiuntivo con la coppia chiave-valore mancante, `ArrayExample.Entries` corrisponde alla matrice di configurazione completa:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1129">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="d97f5-1130">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1130">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="d97f5-1131">In `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1131">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="d97f5-1132">La coppia chiave-valore mostrata nella tabella viene caricata nella configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1132">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="d97f5-1133">Chiave</span><span class="sxs-lookup"><span data-stu-id="d97f5-1133">Key</span></span>             | <span data-ttu-id="d97f5-1134">valore</span><span class="sxs-lookup"><span data-stu-id="d97f5-1134">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="d97f5-1135">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="d97f5-1135">array:entries:3</span></span> | <span data-ttu-id="d97f5-1136">value3</span><span class="sxs-lookup"><span data-stu-id="d97f5-1136">value3</span></span> |

<span data-ttu-id="d97f5-1137">Se l'istanza della classe `ArrayExample` viene associata dopo che il provider di configurazione JSON include la voce per l'indice &num;3, la matrice `ArrayExample.Entries` include il valore.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1137">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="d97f5-1138">Indice di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="d97f5-1138">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="d97f5-1139">`ArrayExample.Entries` Valore</span><span class="sxs-lookup"><span data-stu-id="d97f5-1139">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="d97f5-1140">0</span><span class="sxs-lookup"><span data-stu-id="d97f5-1140">0</span></span>                            | <span data-ttu-id="d97f5-1141">value0</span><span class="sxs-lookup"><span data-stu-id="d97f5-1141">value0</span></span>                       |
| <span data-ttu-id="d97f5-1142">1</span><span class="sxs-lookup"><span data-stu-id="d97f5-1142">1</span></span>                            | <span data-ttu-id="d97f5-1143">value1</span><span class="sxs-lookup"><span data-stu-id="d97f5-1143">value1</span></span>                       |
| <span data-ttu-id="d97f5-1144">2</span><span class="sxs-lookup"><span data-stu-id="d97f5-1144">2</span></span>                            | <span data-ttu-id="d97f5-1145">value2</span><span class="sxs-lookup"><span data-stu-id="d97f5-1145">value2</span></span>                       |
| <span data-ttu-id="d97f5-1146">3</span><span class="sxs-lookup"><span data-stu-id="d97f5-1146">3</span></span>                            | <span data-ttu-id="d97f5-1147">value3</span><span class="sxs-lookup"><span data-stu-id="d97f5-1147">value3</span></span>                       |
| <span data-ttu-id="d97f5-1148">4</span><span class="sxs-lookup"><span data-stu-id="d97f5-1148">4</span></span>                            | <span data-ttu-id="d97f5-1149">value4</span><span class="sxs-lookup"><span data-stu-id="d97f5-1149">value4</span></span>                       |
| <span data-ttu-id="d97f5-1150">5</span><span class="sxs-lookup"><span data-stu-id="d97f5-1150">5</span></span>                            | <span data-ttu-id="d97f5-1151">value5</span><span class="sxs-lookup"><span data-stu-id="d97f5-1151">value5</span></span>                       |

<span data-ttu-id="d97f5-1152">**Elaborazione di matrice JSON**</span><span class="sxs-lookup"><span data-stu-id="d97f5-1152">**JSON array processing**</span></span>

<span data-ttu-id="d97f5-1153">Se un file JSON contiene una matrice, vengono create chiavi di configurazione per gli elementi della matrice con un indice di sezione in base zero.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1153">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="d97f5-1154">Nel file di configurazione seguente, `subsection` è una matrice:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1154">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="d97f5-1155">Il provider di configurazione JSON legge i dati di configurazione nelle coppie chiave-valore seguenti:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1155">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="d97f5-1156">Chiave</span><span class="sxs-lookup"><span data-stu-id="d97f5-1156">Key</span></span>                     | <span data-ttu-id="d97f5-1157">valore</span><span class="sxs-lookup"><span data-stu-id="d97f5-1157">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="d97f5-1158">json_array:key</span><span class="sxs-lookup"><span data-stu-id="d97f5-1158">json_array:key</span></span>          | <span data-ttu-id="d97f5-1159">valueA</span><span class="sxs-lookup"><span data-stu-id="d97f5-1159">valueA</span></span> |
| <span data-ttu-id="d97f5-1160">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="d97f5-1160">json_array:subsection:0</span></span> | <span data-ttu-id="d97f5-1161">valueB</span><span class="sxs-lookup"><span data-stu-id="d97f5-1161">valueB</span></span> |
| <span data-ttu-id="d97f5-1162">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="d97f5-1162">json_array:subsection:1</span></span> | <span data-ttu-id="d97f5-1163">valueC</span><span class="sxs-lookup"><span data-stu-id="d97f5-1163">valueC</span></span> |
| <span data-ttu-id="d97f5-1164">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="d97f5-1164">json_array:subsection:2</span></span> | <span data-ttu-id="d97f5-1165">valueD</span><span class="sxs-lookup"><span data-stu-id="d97f5-1165">valueD</span></span> |

<span data-ttu-id="d97f5-1166">Nell'app di esempio, la classe POCO seguente è disponibile per associare le coppie chiave-valore della configurazione:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1166">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="d97f5-1167">Dopo l'associazione, `JsonArrayExample.Key` contiene il valore `valueA`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1167">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="d97f5-1168">I valori di sottosezione vengono archiviati nella proprietà matrice POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1168">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="d97f5-1169">Indice di `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="d97f5-1169">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="d97f5-1170">`JsonArrayExample.Subsection` Valore</span><span class="sxs-lookup"><span data-stu-id="d97f5-1170">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="d97f5-1171">0</span><span class="sxs-lookup"><span data-stu-id="d97f5-1171">0</span></span>                                   | <span data-ttu-id="d97f5-1172">valueB</span><span class="sxs-lookup"><span data-stu-id="d97f5-1172">valueB</span></span>                              |
| <span data-ttu-id="d97f5-1173">1</span><span class="sxs-lookup"><span data-stu-id="d97f5-1173">1</span></span>                                   | <span data-ttu-id="d97f5-1174">valueC</span><span class="sxs-lookup"><span data-stu-id="d97f5-1174">valueC</span></span>                              |
| <span data-ttu-id="d97f5-1175">2</span><span class="sxs-lookup"><span data-stu-id="d97f5-1175">2</span></span>                                   | <span data-ttu-id="d97f5-1176">valueD</span><span class="sxs-lookup"><span data-stu-id="d97f5-1176">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="d97f5-1177">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="d97f5-1177">Custom configuration provider</span></span>

<span data-ttu-id="d97f5-1178">L'app di esempio dimostra come creare un provider di configurazione di base che legge le coppie chiave-valore di configurazione da un database usando [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="d97f5-1178">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="d97f5-1179">Il provider ha le caratteristiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1179">The provider has the following characteristics:</span></span>

* <span data-ttu-id="d97f5-1180">Il database in memoria di Entity Framework viene usato a scopo dimostrativo.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1180">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="d97f5-1181">Per usare un database che richiede una stringa di connessione, implementare un `ConfigurationBuilder` secondario per fornire la stringa di connessione da un altro provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1181">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="d97f5-1182">Il provider legge una tabella di database in una configurazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1182">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="d97f5-1183">Il provider non esegue una query sul database per ogni chiave.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1183">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="d97f5-1184">Il ricaricamento in caso di modifica non è implementato, quindi l'aggiornamento del database dopo l'avvio dell'app non ha alcun effetto sulla configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1184">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="d97f5-1185">Definire un'entità `EFConfigurationValue` per l'archiviazione dei valori di configurazione nel database.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1185">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="d97f5-1186">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1186">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="d97f5-1187">Aggiungere `EFConfigurationContext` per archiviare i valori configurati e accedervi.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1187">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="d97f5-1188">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1188">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="d97f5-1189">Creare una classe che implementi <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1189">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="d97f5-1190">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1190">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="d97f5-1191">Creare il provider di configurazione personalizzato ereditando da <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1191">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="d97f5-1192">Il provider di configurazione inizializza il database quando è vuoto.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1192">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="d97f5-1193">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1193">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="d97f5-1194">Un metodo di estensione `AddEFConfiguration` consente di aggiungere l'origine di configurazione a un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1194">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="d97f5-1195">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1195">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="d97f5-1196">L'esempio di codice seguente mostra come usare il `EFConfigurationProvider` personalizzato in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1196">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="d97f5-1197">Accedere alla configurazione durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="d97f5-1197">Access configuration during startup</span></span>

<span data-ttu-id="d97f5-1198">Inserire `IConfiguration` nel costruttore `Startup` per accedere ai valori di configurazione in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1198">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d97f5-1199">Per accedere alla configurazione in `Startup.Configure`, inserire `IConfiguration` direttamente nel metodo o usare l'istanza dal costruttore:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1199">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

```csharp
public class Startup
{
    private readonly IConfiguration _config;

    public Startup(IConfiguration config)
    {
        _config = config;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        var value = _config["key"];
    }

    public void Configure(IApplicationBuilder app, IConfiguration config)
    {
        var value = config["key"];
    }
}
```

<span data-ttu-id="d97f5-1200">Per un esempio di accesso alla configurazione usando metodi di servizio di avvio, vedere [Avvio dell'applicazione: Metodi pratici](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="d97f5-1200">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="d97f5-1201">Accedere alla configurazione in una pagina Razor Pages o in una visualizzazione MVC</span><span class="sxs-lookup"><span data-stu-id="d97f5-1201">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="d97f5-1202">Per accedere alle impostazioni di configurazione in una pagina Razor Pages o in una visualizzazione MVC, aggiungere un [direttiva using](xref:mvc/views/razor#using) ([Informazioni di riferimento su C#: direttiva using](/dotnet/csharp/language-reference/keywords/using-directive)) per lo [spazio dei nomi Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) e inserire <xref:Microsoft.Extensions.Configuration.IConfiguration> nella pagina o nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1202">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="d97f5-1203">In una pagina Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1203">In a Razor Pages page:</span></span>

```cshtml
@page
@model IndexModel
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

<span data-ttu-id="d97f5-1204">In una visualizzazione MVC:</span><span class="sxs-lookup"><span data-stu-id="d97f5-1204">In an MVC view:</span></span>

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="d97f5-1205">Aggiungere la configurazione da un assembly esterno</span><span class="sxs-lookup"><span data-stu-id="d97f5-1205">Add configuration from an external assembly</span></span>

<span data-ttu-id="d97f5-1206">Un'implementazione <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> consente l'aggiunta di miglioramenti a un'app all'avvio, da un assembly esterno alla classe `Startup` dell'app.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1206">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="d97f5-1207">Per altre informazioni, vedere <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="d97f5-1207">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d97f5-1208">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d97f5-1208">Additional resources</span></span>

* <xref:fundamentals/configuration/options>

::: moniker-end
