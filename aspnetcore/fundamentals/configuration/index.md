---
title: Configurazione in ASP.NET Core
author: guardrex
description: Informazioni su come usare l'API di configurazione per configurare un'app ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 263f9f7c4c800a74b745fd636196e1e135afc91b
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2019
ms.locfileid: "73033907"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="1c59b-103">Configurazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1c59b-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="1c59b-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1c59b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1c59b-105">La configurazione delle app in ASP.NET Core si basa su coppie chiave-valore stabilite dai *provider di configurazione*.</span><span class="sxs-lookup"><span data-stu-id="1c59b-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="1c59b-106">I provider di configurazione leggono i dati di configurazione in coppie chiave-valore da un'ampia gamma di origini di configurazione:</span><span class="sxs-lookup"><span data-stu-id="1c59b-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="1c59b-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="1c59b-107">Azure Key Vault</span></span>
* <span data-ttu-id="1c59b-108">Configurazione di app Azure</span><span class="sxs-lookup"><span data-stu-id="1c59b-108">Azure App Configuration</span></span>
* <span data-ttu-id="1c59b-109">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="1c59b-109">Command-line arguments</span></span>
* <span data-ttu-id="1c59b-110">Provider personalizzati (installati o creati)</span><span class="sxs-lookup"><span data-stu-id="1c59b-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="1c59b-111">File della directory</span><span class="sxs-lookup"><span data-stu-id="1c59b-111">Directory files</span></span>
* <span data-ttu-id="1c59b-112">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="1c59b-112">Environment variables</span></span>
* <span data-ttu-id="1c59b-113">Oggetti .NET in memoria</span><span class="sxs-lookup"><span data-stu-id="1c59b-113">In-memory .NET objects</span></span>
* <span data-ttu-id="1c59b-114">File di impostazioni</span><span class="sxs-lookup"><span data-stu-id="1c59b-114">Settings files</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1c59b-115">I pacchetti di configurazione per gli scenari di provider di configurazione comuni ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) sono inclusi in modo implicito dal framework.</span><span class="sxs-lookup"><span data-stu-id="1c59b-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1c59b-116">I pacchetti di configurazione per gli scenari di provider di configurazione comuni ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) sono inclusi nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1c59b-116">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="1c59b-117">Gli esempi di codice che seguono e quelli inclusi nell'app di esempio usano lo spazio dei nomi <xref:Microsoft.Extensions.Configuration>:</span><span class="sxs-lookup"><span data-stu-id="1c59b-117">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="1c59b-118">Il *modello di opzioni* è un'estensione dei concetti di configurazione descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="1c59b-118">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="1c59b-119">Le opzioni usano le classi per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="1c59b-119">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="1c59b-120">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="1c59b-120">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="1c59b-121">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1c59b-121">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="1c59b-122">Host e configurazione delle app</span><span class="sxs-lookup"><span data-stu-id="1c59b-122">Host versus app configuration</span></span>

<span data-ttu-id="1c59b-123">Prima che l'app venga configurata e avviata, viene configurato e avviato un *host*.</span><span class="sxs-lookup"><span data-stu-id="1c59b-123">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="1c59b-124">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="1c59b-124">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="1c59b-125">Sia l'app che l'host vengono configurati tramite i provider di configurazione descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="1c59b-125">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="1c59b-126">Anche le coppie chiave-valore di configurazione dell'host sono incluse nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="1c59b-126">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="1c59b-127">Per altre informazioni su come vengono usati i provider di configurazione quando viene creato l'host e sugli effetti delle origini di configurazione sull'host di configurazione, vedere <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="1c59b-127">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="1c59b-128">Configurazione predefinita</span><span class="sxs-lookup"><span data-stu-id="1c59b-128">Default configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1c59b-129">Le app basate sui modelli [dotnet new](/dotnet/core/tools/dotnet-new) di ASP.NET Core chiamano <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> durante la compilazione di un host.</span><span class="sxs-lookup"><span data-stu-id="1c59b-129">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="1c59b-130">`CreateDefaultBuilder` fornisce la configurazione predefinita per l'app nell'ordine seguente:</span><span class="sxs-lookup"><span data-stu-id="1c59b-130">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="1c59b-131">La configurazione seguente si applica alle app che usano l'[host generico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="1c59b-131">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="1c59b-132">Per informazioni dettagliate sulla configurazione predefinita quando viene usato l'[host Web](xref:fundamentals/host/web-host), vedere la [versione di questo argomento per ASP.NET Core 2.2](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="1c59b-132">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="1c59b-133">La configurazione dell'host viene specificata da:</span><span class="sxs-lookup"><span data-stu-id="1c59b-133">Host configuration is provided from:</span></span>
  * <span data-ttu-id="1c59b-134">Variabili di ambiente con prefisso `DOTNET_` (ad esempio `DOTNET_ENVIRONMENT`) che usano il [provider di configurazione delle variabili di ambiente](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c59b-134">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="1c59b-135">Il prefisso (`DOTNET_`) viene rimosso al caricamento delle coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-135">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="1c59b-136">Argomenti della riga di comando che usano il [provider di configurazione della riga di comando](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c59b-136">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="1c59b-137">La configurazione predefinita dell'host Web viene stabilita (`ConfigureWebHostDefaults`) nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="1c59b-137">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="1c59b-138">Kestrel viene usato come server Web e configurato con i provider di configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="1c59b-138">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="1c59b-139">Aggiungere il middleware di filtro host.</span><span class="sxs-lookup"><span data-stu-id="1c59b-139">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="1c59b-140">Aggiungere il middleware delle intestazioni inoltrate se la variabile di ambiente `ASPNETCORE_FORWARDEDHEADERS_ENABLED` è impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-140">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="1c59b-141">Abilitare l'integrazione di IIS.</span><span class="sxs-lookup"><span data-stu-id="1c59b-141">Enable IIS integration.</span></span>
* <span data-ttu-id="1c59b-142">La configurazione dell'app viene fornita da:</span><span class="sxs-lookup"><span data-stu-id="1c59b-142">App configuration is provided from:</span></span>
  * <span data-ttu-id="1c59b-143">*appsettings.json* mediante il [provider di configurazione dei file](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c59b-143">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="1c59b-144">*appsettings.{Ambiente}.json* mediante il [provider di configurazione dei file](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c59b-144">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="1c59b-145">[Strumento di gestione dei segreti](xref:security/app-secrets) quando l'app viene eseguita nell'ambiente `Development` usando l'assembly di ingresso.</span><span class="sxs-lookup"><span data-stu-id="1c59b-145">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="1c59b-146">Variabili di ambiente che usano il [provider di configurazione delle variabili di ambiente](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c59b-146">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="1c59b-147">Argomenti della riga di comando che usano il [provider di configurazione della riga di comando](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c59b-147">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1c59b-148">Le app basate sui modelli [dotnet new](/dotnet/core/tools/dotnet-new) di ASP.NET Core chiamano <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> durante la compilazione di un host.</span><span class="sxs-lookup"><span data-stu-id="1c59b-148">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="1c59b-149">`CreateDefaultBuilder` fornisce la configurazione predefinita per l'app nell'ordine seguente:</span><span class="sxs-lookup"><span data-stu-id="1c59b-149">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="1c59b-150">La configurazione seguente si applica alle app che usano l'[host Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="1c59b-150">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="1c59b-151">Per informazioni dettagliate sulla configurazione predefinita quando viene usato l'[host generico](xref:fundamentals/host/generic-host), vedere la [versione più recente di questo argomento](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="1c59b-151">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="1c59b-152">La configurazione dell'host viene specificata da:</span><span class="sxs-lookup"><span data-stu-id="1c59b-152">Host configuration is provided from:</span></span>
  * <span data-ttu-id="1c59b-153">Variabili di ambiente con prefisso `ASPNETCORE_` (ad esempio `ASPNETCORE_ENVIRONMENT`) che usano il [provider di configurazione delle variabili di ambiente](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c59b-153">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="1c59b-154">Il prefisso (`ASPNETCORE_`) viene rimosso al caricamento delle coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-154">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="1c59b-155">Argomenti della riga di comando che usano il [provider di configurazione della riga di comando](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c59b-155">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="1c59b-156">La configurazione dell'app viene fornita da:</span><span class="sxs-lookup"><span data-stu-id="1c59b-156">App configuration is provided from:</span></span>
  * <span data-ttu-id="1c59b-157">*appsettings.json* mediante il [provider di configurazione dei file](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c59b-157">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="1c59b-158">*appsettings.{Ambiente}.json* mediante il [provider di configurazione dei file](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c59b-158">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="1c59b-159">[Strumento di gestione dei segreti](xref:security/app-secrets) quando l'app viene eseguita nell'ambiente `Development` usando l'assembly di ingresso.</span><span class="sxs-lookup"><span data-stu-id="1c59b-159">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="1c59b-160">Variabili di ambiente che usano il [provider di configurazione delle variabili di ambiente](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c59b-160">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="1c59b-161">Argomenti della riga di comando che usano il [provider di configurazione della riga di comando](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1c59b-161">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

## <a name="security"></a><span data-ttu-id="1c59b-162">Sicurezza</span><span class="sxs-lookup"><span data-stu-id="1c59b-162">Security</span></span>

<span data-ttu-id="1c59b-163">Per proteggere i dati di configurazione sensibili, adottare le pratiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="1c59b-163">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="1c59b-164">Non archiviare mai la password o altri dati sensibili nel codice del provider di configurazione o in file di configurazione di testo normale.</span><span class="sxs-lookup"><span data-stu-id="1c59b-164">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="1c59b-165">Non usare i segreti di produzione in ambienti di sviluppo o di test.</span><span class="sxs-lookup"><span data-stu-id="1c59b-165">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="1c59b-166">Specificare i segreti all'esterno del progetto in modo che non possano essere inavvertitamente inviati a un repository del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="1c59b-166">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="1c59b-167">Per altre informazioni, vedere i seguenti argomenti:</span><span class="sxs-lookup"><span data-stu-id="1c59b-167">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="1c59b-168"><xref:security/app-secrets> &ndash; include consigli sull'uso delle variabili di ambiente per archiviare i dati sensibili.</span><span class="sxs-lookup"><span data-stu-id="1c59b-168"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="1c59b-169">Secret Manager usa il provider di configurazione dei file per archiviare i segreti utente in un file JSON nel sistema locale.</span><span class="sxs-lookup"><span data-stu-id="1c59b-169">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="1c59b-170">Il provider di configurazione dei file è descritto più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="1c59b-170">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="1c59b-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) archivia in modo sicuro i segreti delle app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c59b-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="1c59b-172">Per ulteriori informazioni, vedere <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="1c59b-172">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="1c59b-173">Dati di configurazione gerarchici</span><span class="sxs-lookup"><span data-stu-id="1c59b-173">Hierarchical configuration data</span></span>

<span data-ttu-id="1c59b-174">L'API di configurazione è in grado di mantenere i dati di configurazione gerarchici rendendoli flat grazie all'uso di un delimitatore nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-174">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="1c59b-175">Nel file JSON seguente, esistono quattro chiavi in una gerarchia strutturata di due sezioni:</span><span class="sxs-lookup"><span data-stu-id="1c59b-175">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="1c59b-176">Quando il file viene letto nella configurazione, vengono create chiavi univoche per mantenere la struttura di dati gerarchici originale dell'origine di configurazione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-176">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="1c59b-177">Le sezioni e le chiavi vengono rese flat usando due punti (`:`) per mantenere la struttura originale:</span><span class="sxs-lookup"><span data-stu-id="1c59b-177">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="1c59b-178">section0:key0</span><span class="sxs-lookup"><span data-stu-id="1c59b-178">section0:key0</span></span>
* <span data-ttu-id="1c59b-179">section0:key1</span><span class="sxs-lookup"><span data-stu-id="1c59b-179">section0:key1</span></span>
* <span data-ttu-id="1c59b-180">section1:key0</span><span class="sxs-lookup"><span data-stu-id="1c59b-180">section1:key0</span></span>
* <span data-ttu-id="1c59b-181">section1:key1</span><span class="sxs-lookup"><span data-stu-id="1c59b-181">section1:key1</span></span>

<span data-ttu-id="1c59b-182">I metodi <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> e <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> sono disponibili per isolare le sezioni e gli elementi figlio di una sezione nei dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-182"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="1c59b-183">Questi metodi sono descritti più avanti in [GetSection, GetChildren ed Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="1c59b-183">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="1c59b-184">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="1c59b-184">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="1c59b-185">Origini e provider</span><span class="sxs-lookup"><span data-stu-id="1c59b-185">Sources and providers</span></span>

<span data-ttu-id="1c59b-186">All'avvio dell'app, le origini di configurazione vengono lette nell'ordine con cui sono specificati i rispettivi provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-186">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="1c59b-187">I provider di configurazione che implementano il rilevamento delle modifiche sono in grado di ricaricare la configurazione quando viene modificata un'impostazione sottostante.</span><span class="sxs-lookup"><span data-stu-id="1c59b-187">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="1c59b-188">Ad esempio, il provider di configurazione dei file (descritto più avanti in questo argomento) e il [provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration) implementano il rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="1c59b-188">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="1c59b-189"><xref:Microsoft.Extensions.Configuration.IConfiguration> è disponibile nel contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) dell'app.</span><span class="sxs-lookup"><span data-stu-id="1c59b-189"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="1c59b-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> può essere inserito in <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> di Razor Pages per ottenere la configurazione per la classe:</span><span class="sxs-lookup"><span data-stu-id="1c59b-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    // The _config local variable is used to obtain configuration 
    // throughout the class.
}
```

<span data-ttu-id="1c59b-191">I provider di configurazione non possono usare l'inserimento delle dipendenze, perché non è disponibile quando vengono configurati dall'host.</span><span class="sxs-lookup"><span data-stu-id="1c59b-191">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="1c59b-192">Tasti</span><span class="sxs-lookup"><span data-stu-id="1c59b-192">Keys</span></span>

<span data-ttu-id="1c59b-193">Le chiavi di configurazione adottano le convenzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="1c59b-193">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="1c59b-194">Per le chiavi non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="1c59b-194">Keys are case-insensitive.</span></span> <span data-ttu-id="1c59b-195">Ad esempio, `ConnectionString` e `connectionstring` vengono considerate chiavi equivalenti.</span><span class="sxs-lookup"><span data-stu-id="1c59b-195">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="1c59b-196">Se un valore per la stessa chiave viene impostato dallo stesso provider di configurazione o da provider diversi, viene usato l'ultimo valore impostato per la chiave.</span><span class="sxs-lookup"><span data-stu-id="1c59b-196">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="1c59b-197">Chiavi gerarchiche</span><span class="sxs-lookup"><span data-stu-id="1c59b-197">Hierarchical keys</span></span>
  * <span data-ttu-id="1c59b-198">Nell'ambito dell'API di configurazione, il separatore due punti (`:`) funziona in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="1c59b-198">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="1c59b-199">Nelle variabili di ambiente, un separatore due punti potrebbe non funzionare in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="1c59b-199">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="1c59b-200">Il doppio carattere di sottolineatura (`__`) è supportato da tutte le piattaforme e viene convertito automaticamente nei due punti.</span><span class="sxs-lookup"><span data-stu-id="1c59b-200">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="1c59b-201">In Azure Key Vault, le chiavi gerarchiche usano `--` (due trattini) come separatore.</span><span class="sxs-lookup"><span data-stu-id="1c59b-201">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="1c59b-202">È necessario fornire il codice per sostituire i trattini con due punti quando i segreti vengono caricati nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="1c59b-202">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="1c59b-203">Il <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supporta l'associazione di matrici agli oggetti usando gli indici delle matrici nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-203">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="1c59b-204">L'associazione di matrici è descritta nella sezione [Associare una matrice a una classe](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="1c59b-204">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="1c59b-205">Valori</span><span class="sxs-lookup"><span data-stu-id="1c59b-205">Values</span></span>

<span data-ttu-id="1c59b-206">I valori di configurazione adottano le convenzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="1c59b-206">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="1c59b-207">I valori sono stringhe.</span><span class="sxs-lookup"><span data-stu-id="1c59b-207">Values are strings.</span></span>
* <span data-ttu-id="1c59b-208">I valori null non possono essere archiviati nella configurazione o associati a oggetti.</span><span class="sxs-lookup"><span data-stu-id="1c59b-208">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="1c59b-209">Provider</span><span class="sxs-lookup"><span data-stu-id="1c59b-209">Providers</span></span>

<span data-ttu-id="1c59b-210">La tabella seguente mostra i provider di configurazione disponibili per le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1c59b-210">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="1c59b-211">Provider</span><span class="sxs-lookup"><span data-stu-id="1c59b-211">Provider</span></span> | <span data-ttu-id="1c59b-212">Fornisce la configurazione da&hellip;</span><span class="sxs-lookup"><span data-stu-id="1c59b-212">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="1c59b-213">[Provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="1c59b-213">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="1c59b-214">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="1c59b-214">Azure Key Vault</span></span> |
| <span data-ttu-id="1c59b-215">[Provider di Configurazione app](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Documentazione di Azure)</span><span class="sxs-lookup"><span data-stu-id="1c59b-215">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="1c59b-216">Configurazione di app Azure</span><span class="sxs-lookup"><span data-stu-id="1c59b-216">Azure App Configuration</span></span> |
| [<span data-ttu-id="1c59b-217">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="1c59b-217">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="1c59b-218">Parametri della riga di comando</span><span class="sxs-lookup"><span data-stu-id="1c59b-218">Command-line parameters</span></span> |
| [<span data-ttu-id="1c59b-219">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="1c59b-219">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="1c59b-220">Origine personalizzata</span><span class="sxs-lookup"><span data-stu-id="1c59b-220">Custom source</span></span> |
| [<span data-ttu-id="1c59b-221">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="1c59b-221">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="1c59b-222">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="1c59b-222">Environment variables</span></span> |
| [<span data-ttu-id="1c59b-223">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="1c59b-223">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="1c59b-224">File (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="1c59b-224">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="1c59b-225">Provider di configurazione chiave-per-file</span><span class="sxs-lookup"><span data-stu-id="1c59b-225">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="1c59b-226">File della directory</span><span class="sxs-lookup"><span data-stu-id="1c59b-226">Directory files</span></span> |
| [<span data-ttu-id="1c59b-227">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="1c59b-227">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="1c59b-228">Raccolte in memoria</span><span class="sxs-lookup"><span data-stu-id="1c59b-228">In-memory collections</span></span> |
| <span data-ttu-id="1c59b-229">[Segreti utente (Secret Manager)](xref:security/app-secrets) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="1c59b-229">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="1c59b-230">File nella directory dei profili utente</span><span class="sxs-lookup"><span data-stu-id="1c59b-230">File in the user profile directory</span></span> |

<span data-ttu-id="1c59b-231">Le origini di configurazione vengono lette nell'ordine in cui vengono specificati i rispetti provider di configurazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="1c59b-231">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="1c59b-232">I provider di configurazione descritti in questo argomento vengono presentati in ordine alfabetico e non nell'ordine in cui potrebbe disporli il codice.</span><span class="sxs-lookup"><span data-stu-id="1c59b-232">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="1c59b-233">Ordinare i provider di configurazione nel codice in base alle specifiche priorità per le origini di configurazione sottostanti.</span><span class="sxs-lookup"><span data-stu-id="1c59b-233">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="1c59b-234">Una sequenza tipica di provider di configurazione è:</span><span class="sxs-lookup"><span data-stu-id="1c59b-234">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="1c59b-235">File (*appsettings.json*, *appsettings.{Environment}.json*, dove `{Environment}` è l'ambiente di hosting corrente dell'app)</span><span class="sxs-lookup"><span data-stu-id="1c59b-235">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="1c59b-236">Insieme di credenziali delle chiavi di Azure</span><span class="sxs-lookup"><span data-stu-id="1c59b-236">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="1c59b-237">[Segreti utente (Secret Manager)](xref:security/app-secrets) (solo nell'ambiente di sviluppo)</span><span class="sxs-lookup"><span data-stu-id="1c59b-237">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="1c59b-238">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="1c59b-238">Environment variables</span></span>
1. <span data-ttu-id="1c59b-239">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="1c59b-239">Command-line arguments</span></span>

<span data-ttu-id="1c59b-240">È pratica comune posizionare il provider di configurazione della riga di comando per ultimo in una serie di provider per consentire agli argomenti della riga di comando di sostituire la configurazione impostata da altri provider.</span><span class="sxs-lookup"><span data-stu-id="1c59b-240">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="1c59b-241">La sequenza di provider precedente viene usata quando si inizializza un nuovo generatore di host con `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-241">The preceding sequence of providers is used when you initialize a new host builder with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="1c59b-242">Per altre informazioni, vedere la sezione [Configurazione predefinita](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="1c59b-242">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="1c59b-243">Configurare il generatore di host con ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="1c59b-243">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="1c59b-244">Per configurare il generatore di host, chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> e specificare la configurazione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-244">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="1c59b-245">`ConfigureHostConfiguration` viene usato per inizializzare l'oggetto <xref:Microsoft.Extensions.Hosting.IHostEnvironment> da usare in un secondo momento nel processo di compilazione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-245">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="1c59b-246">`ConfigureHostConfiguration` può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="1c59b-246">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="1c59b-247">Configurare il generatore di host con UseConfiguration</span><span class="sxs-lookup"><span data-stu-id="1c59b-247">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="1c59b-248">Per configurare il generatore di host, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> sul generatore di host con la configurazione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-248">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

::: moniker-end

## <a name="configureappconfiguration"></a><span data-ttu-id="1c59b-249">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="1c59b-249">ConfigureAppConfiguration</span></span>

<span data-ttu-id="1c59b-250">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare i provider di configurazione dell'app, oltre a quelli aggiunti automaticamente da `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="1c59b-250">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="1c59b-251">Sostituire la configurazione precedente con argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="1c59b-251">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="1c59b-252">Per fornire la configurazione dell'app che può essere sostituita con argomenti della riga di comando, chiamare `AddCommandLine` per ultimo:</span><span class="sxs-lookup"><span data-stu-id="1c59b-252">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="1c59b-253">Utilizzare la configurazione durante l'avvio dell'app</span><span class="sxs-lookup"><span data-stu-id="1c59b-253">Consume configuration during app startup</span></span>

<span data-ttu-id="1c59b-254">La configurazione fornita all'app in `ConfigureAppConfiguration` è disponibile durante l'avvio dell'app, incluso `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-254">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1c59b-255">Per altre informazioni, vedere la sezione [Accedere alla configurazione durante l'avvio](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="1c59b-255">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="1c59b-256">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="1c59b-256">Command-line Configuration Provider</span></span>

<span data-ttu-id="1c59b-257"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> carica la configurazione da coppie chiave-valore di argomenti della riga di comando in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-257">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="1c59b-258">Per attivare la configurazione della riga di comando, metodo di estensione <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> viene chiamato su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1c59b-258">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1c59b-259">`AddCommandLine` viene chiamato automaticamente quando viene chiamato il metodo `CreateDefaultBuilder(string [])`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-259">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="1c59b-260">Per altre informazioni, vedere la sezione [Configurazione predefinita](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="1c59b-260">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="1c59b-261">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="1c59b-261">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="1c59b-262">Una configurazione facoltativa dai file *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="1c59b-262">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="1c59b-263">[Segreti utente (Secret Manager)](xref:security/app-secrets) nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="1c59b-263">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="1c59b-264">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="1c59b-264">Environment variables.</span></span>

<span data-ttu-id="1c59b-265">`CreateDefaultBuilder` aggiunge il provider di configurazione della riga di comando per ultimo.</span><span class="sxs-lookup"><span data-stu-id="1c59b-265">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="1c59b-266">Gli argomenti della riga di comando passati in fase di esecuzione sostituiscono la configurazione impostata dagli altri provider.</span><span class="sxs-lookup"><span data-stu-id="1c59b-266">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="1c59b-267">`CreateDefaultBuilder` opera durante la creazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="1c59b-267">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="1c59b-268">Pertanto, la configurazione della riga di comando attivata da `CreateDefaultBuilder` può influire sul modo in cui l'host viene configurato.</span><span class="sxs-lookup"><span data-stu-id="1c59b-268">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="1c59b-269">Per le app basate sui modelli di ASP.NET Core, `AddCommandLine` è già stato chiamato da `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-269">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="1c59b-270">Per aggiungere altri provider di configurazione e mantenere la possibilità di sostituire la configurazione di tali provider con argomenti della riga di comando, chiamare i provider aggiuntivi dell'app in `ConfigureAppConfiguration` e quindi chiamare `AddCommandLine` per ultimo.</span><span class="sxs-lookup"><span data-stu-id="1c59b-270">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="1c59b-271">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="1c59b-271">**Example**</span></span>

<span data-ttu-id="1c59b-272">L'app di esempio consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include una chiamata a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="1c59b-272">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="1c59b-273">Aprire un prompt dei comandi nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="1c59b-273">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="1c59b-274">Fornire un argomento della riga di comando per il comando `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-274">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="1c59b-275">Dopo che l'app è in esecuzione, aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-275">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="1c59b-276">Notare che l'output contiene la coppia chiave-valore per l'argomento della riga di comando di configurazione fornito per `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-276">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="1c59b-277">argomenti</span><span class="sxs-lookup"><span data-stu-id="1c59b-277">Arguments</span></span>

<span data-ttu-id="1c59b-278">Il valore deve seguire un segno di uguale (`=`) o la chiave deve avere un prefisso (`--` o `/`) quando il valore segue uno spazio.</span><span class="sxs-lookup"><span data-stu-id="1c59b-278">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="1c59b-279">Il valore non è necessario se viene usato un segno di uguale, ad esempio `CommandLineKey=`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-279">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="1c59b-280">Prefisso chiave</span><span class="sxs-lookup"><span data-stu-id="1c59b-280">Key prefix</span></span>               | <span data-ttu-id="1c59b-281">Esempio</span><span class="sxs-lookup"><span data-stu-id="1c59b-281">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="1c59b-282">Nessun prefisso</span><span class="sxs-lookup"><span data-stu-id="1c59b-282">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="1c59b-283">Due trattini (`--`)</span><span class="sxs-lookup"><span data-stu-id="1c59b-283">Two dashes (`--`)</span></span>        | <span data-ttu-id="1c59b-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="1c59b-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="1c59b-285">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="1c59b-285">Forward slash (`/`)</span></span>      | <span data-ttu-id="1c59b-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="1c59b-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="1c59b-287">Nello stesso comando, non mischiare coppie chiave-valore di argomenti della riga di comando che usano un segno di uguale con coppie chiave-valore che usano uno spazio.</span><span class="sxs-lookup"><span data-stu-id="1c59b-287">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="1c59b-288">Comandi di esempio:</span><span class="sxs-lookup"><span data-stu-id="1c59b-288">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="1c59b-289">Mapping di sostituzione</span><span class="sxs-lookup"><span data-stu-id="1c59b-289">Switch mappings</span></span>

<span data-ttu-id="1c59b-290">I mapping di sostituzione consentono di usare la logica di sostituzione del nome della chiave.</span><span class="sxs-lookup"><span data-stu-id="1c59b-290">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="1c59b-291">Quando si crea manualmente la configurazione con un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, è possibile specificare un dizionario di mapping di sostituzione per il metodo <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="1c59b-291">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="1c59b-292">Quando viene utilizzato il dizionario dei mapping di sostituzione, nel dizionario viene controllata la presenza di una chiave corrispondente alla chiave fornita da un argomento della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="1c59b-292">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="1c59b-293">Se la chiave della riga di comando viene trovata nel dizionario, il valore del dizionario (la sostituzione della chiave) viene passato nuovamente per impostare la coppia chiave-valore nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="1c59b-293">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="1c59b-294">Un mapping di sostituzione è necessario per le chiavi della riga di comando con un trattino singolo (`-`) come prefisso.</span><span class="sxs-lookup"><span data-stu-id="1c59b-294">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="1c59b-295">Regole principali del dizionario dei mapping di sostituzione:</span><span class="sxs-lookup"><span data-stu-id="1c59b-295">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="1c59b-296">Le sostituzioni devono iniziare con un trattino (`-`) o un trattino doppio (`--`).</span><span class="sxs-lookup"><span data-stu-id="1c59b-296">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="1c59b-297">Il dizionario dei mapping di sostituzione non deve contenere chiavi duplicate.</span><span class="sxs-lookup"><span data-stu-id="1c59b-297">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="1c59b-298">Creare un dizionario dei mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-298">Create a switch mappings dictionary.</span></span> <span data-ttu-id="1c59b-299">Nell'esempio seguente vengono creati due mapping di sostituzione:</span><span class="sxs-lookup"><span data-stu-id="1c59b-299">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="1c59b-300">Quando viene creato l'host, chiamare `AddCommandLine` con il dizionario dei mapping di sostituzione:</span><span class="sxs-lookup"><span data-stu-id="1c59b-300">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="1c59b-301">Per le app che usano i mapping di sostituzione, la chiamata a `CreateDefaultBuilder` non deve passare argomenti.</span><span class="sxs-lookup"><span data-stu-id="1c59b-301">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="1c59b-302">La chiamata `AddCommandLine` del metodo `CreateDefaultBuilder` non include sostituzioni mappate e non è possibile passare il dizionario dei mapping di sostituzione a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-302">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="1c59b-303">La soluzione consiste nel non passare gli argomenti a `CreateDefaultBuilder` consentendo invece al metodo `AddCommandLine` del metodo `ConfigurationBuilder` di elaborare entrambi gli argomenti e il dizionario dei mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-303">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="1c59b-304">Il dizionario dei mapping di sostituzione creato contiene i dati visualizzati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="1c59b-304">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="1c59b-305">Chiave</span><span class="sxs-lookup"><span data-stu-id="1c59b-305">Key</span></span>       | <span data-ttu-id="1c59b-306">Value</span><span class="sxs-lookup"><span data-stu-id="1c59b-306">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="1c59b-307">Se le chiavi con mapping di sostituzione vengono usate all'avvio dell'app, la configurazione riceve il valore di configurazione per la chiave fornita dal dizionario:</span><span class="sxs-lookup"><span data-stu-id="1c59b-307">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="1c59b-308">Dopo aver eseguito il comando precedente, la configurazione contiene i valori mostrati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="1c59b-308">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="1c59b-309">Chiave</span><span class="sxs-lookup"><span data-stu-id="1c59b-309">Key</span></span>               | <span data-ttu-id="1c59b-310">Value</span><span class="sxs-lookup"><span data-stu-id="1c59b-310">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="1c59b-311">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="1c59b-311">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="1c59b-312"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> carica la configurazione da coppie chiave-valore di variabili di ambiente in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-312">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="1c59b-313">Per attivare la configurazione delle variabili di ambiente, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1c59b-313">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="1c59b-314">[Servizio App di Azure](https://azure.microsoft.com/services/app-service/) consente di impostare variabili di ambiente nel portale di Azure che possono sostituire la configurazione delle app tramite il provider di configurazione delle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="1c59b-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="1c59b-315">Per altre informazioni, vedere [App di Azure: Eseguire l'override della configurazione delle app usando il portale di Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="1c59b-315">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1c59b-316">`AddEnvironmentVariables` consente di caricare variabili di ambiente con prefisso `DOTNET_` per la [configurazione host](#host-versus-app-configuration) quando viene inizializzato un nuovo generatore di host con l'[host generico](xref:fundamentals/host/generic-host) e viene chiamato `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-316">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="1c59b-317">Per altre informazioni, vedere la sezione [Configurazione predefinita](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="1c59b-317">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1c59b-318">`AddEnvironmentVariables` consente di caricare variabili di ambiente con prefisso `ASPNETCORE_` per la [configurazione host](#host-versus-app-configuration) quando viene inizializzato un nuovo generatore di host con l'[host Web](xref:fundamentals/host/web-host) e viene chiamato `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-318">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="1c59b-319">Per altre informazioni, vedere la sezione [Configurazione predefinita](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="1c59b-319">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

<span data-ttu-id="1c59b-320">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="1c59b-320">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="1c59b-321">Configurazione delle app dalle variabili di ambiente senza prefisso chiamando `AddEnvironmentVariables` senza prefisso.</span><span class="sxs-lookup"><span data-stu-id="1c59b-321">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="1c59b-322">Una configurazione facoltativa dai file *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="1c59b-322">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="1c59b-323">[Segreti utente (Secret Manager)](xref:security/app-secrets) nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="1c59b-323">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="1c59b-324">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="1c59b-324">Command-line arguments.</span></span>

<span data-ttu-id="1c59b-325">Il provider di configurazione delle variabili di ambiente viene chiamato dopo aver stabilito la configurazione dai segreti utente e dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="1c59b-325">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="1c59b-326">La chiamata del provider in questa posizione consente alle variabili di ambiente lette in fase di esecuzione di sostituire la configurazione impostata dai segreti utente e dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="1c59b-326">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="1c59b-327">Se è necessario specificare la configurazione dell'app da variabili di ambiente aggiuntive, chiamare i provider aggiuntivi dell'app in `ConfigureAppConfiguration` e chiamare `AddEnvironmentVariables` con il prefisso.</span><span class="sxs-lookup"><span data-stu-id="1c59b-327">If you need to provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call additional providers here as needed.
    // Call AddEnvironmentVariables last if you need to allow
    // environment variables to override values from other 
    // providers.
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
}
```

<span data-ttu-id="1c59b-328">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="1c59b-328">**Example**</span></span>

<span data-ttu-id="1c59b-329">L'app di esempio consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include una chiamata a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-329">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="1c59b-330">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="1c59b-330">Run the sample app.</span></span> <span data-ttu-id="1c59b-331">Aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-331">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="1c59b-332">Notare che l'output contiene la coppia chiave-valore per la variabile di ambiente `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-332">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="1c59b-333">Il valore riflette l'ambiente in cui l'app è in esecuzione, in genere `Development` durante l'esecuzione locale.</span><span class="sxs-lookup"><span data-stu-id="1c59b-333">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="1c59b-334">Per limitare l'elenco delle variabili di ambiente restituito dall'app, l'app filtra le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="1c59b-334">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="1c59b-335">Vedere il file *Pages/Index.cshtml.cs* dell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="1c59b-335">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="1c59b-336">Se si desidera esporre tutte le variabili di ambiente disponibili all'app, modificare `FilteredConfiguration` in *Pages/Index.cshtml.cs* come segue:</span><span class="sxs-lookup"><span data-stu-id="1c59b-336">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="1c59b-337">Prefissi</span><span class="sxs-lookup"><span data-stu-id="1c59b-337">Prefixes</span></span>

<span data-ttu-id="1c59b-338">Le variabili di ambiente caricate nella configurazione dell'app vengono filtrate quando si specifica un prefisso per il metodo `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-338">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="1c59b-339">Ad esempio, per filtrare le variabili di ambiente in base al prefisso `CUSTOM_`, fornire il prefisso al provider di configurazione:</span><span class="sxs-lookup"><span data-stu-id="1c59b-339">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="1c59b-340">Il prefisso viene rimosso quando vengono create le coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-340">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="1c59b-341">Quando viene creato il generatore di host, la configurazione dell'host viene fornita dalle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="1c59b-341">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="1c59b-342">Per altre informazioni sul prefisso usato per queste variabili di ambiente, vedere la sezione [Configurazione predefinita](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="1c59b-342">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="1c59b-343">**Prefissi della stringa di connessione**</span><span class="sxs-lookup"><span data-stu-id="1c59b-343">**Connection string prefixes**</span></span>

<span data-ttu-id="1c59b-344">L'API di configurazione prevede regole di elaborazione speciali per quattro variabili di ambiente di stringa di connessione coinvolte nella configurazione di stringhe di connessione di Azure per l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="1c59b-344">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="1c59b-345">Le variabili di ambiente con i prefissi indicati nella tabella vengono caricate nell'app se non viene fornito alcun prefisso a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-345">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="1c59b-346">Prefisso della stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="1c59b-346">Connection string prefix</span></span> | <span data-ttu-id="1c59b-347">Provider</span><span class="sxs-lookup"><span data-stu-id="1c59b-347">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="1c59b-348">Provider personalizzato</span><span class="sxs-lookup"><span data-stu-id="1c59b-348">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="1c59b-349">MySQL</span><span class="sxs-lookup"><span data-stu-id="1c59b-349">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="1c59b-350">Database SQL di Azure</span><span class="sxs-lookup"><span data-stu-id="1c59b-350">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="1c59b-351">SQL Server</span><span class="sxs-lookup"><span data-stu-id="1c59b-351">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="1c59b-352">Quando una variabile di ambiente viene individuata e caricata nella configurazione con uno qualsiasi dei quattro prefissi indicati nella tabella:</span><span class="sxs-lookup"><span data-stu-id="1c59b-352">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="1c59b-353">La chiave di configurazione viene creata rimuovendo il prefisso della variabile di ambiente e aggiungendo una sezione per la chiave di configurazione (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="1c59b-353">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="1c59b-354">Viene creata una nuova coppia chiave-valore della configurazione che rappresenta il provider di connessione al database (ad eccezione di `CUSTOMCONNSTR_`, che non ha un provider dichiarato).</span><span class="sxs-lookup"><span data-stu-id="1c59b-354">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="1c59b-355">Chiave della variabile di ambiente</span><span class="sxs-lookup"><span data-stu-id="1c59b-355">Environment variable key</span></span> | <span data-ttu-id="1c59b-356">Chiave di configurazione convertita</span><span class="sxs-lookup"><span data-stu-id="1c59b-356">Converted configuration key</span></span> | <span data-ttu-id="1c59b-357">Voce di configurazione del provider</span><span class="sxs-lookup"><span data-stu-id="1c59b-357">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="1c59b-358">Voce di configurazione non creata.</span><span class="sxs-lookup"><span data-stu-id="1c59b-358">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="1c59b-359">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="1c59b-359">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="1c59b-360">Valore: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="1c59b-360">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="1c59b-361">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="1c59b-361">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="1c59b-362">Valore: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="1c59b-362">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="1c59b-363">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="1c59b-363">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="1c59b-364">Valore: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="1c59b-364">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="1c59b-365">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="1c59b-365">File Configuration Provider</span></span>

<span data-ttu-id="1c59b-366"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> è la classe base per il caricamento della configurazione dal file system.</span><span class="sxs-lookup"><span data-stu-id="1c59b-366"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="1c59b-367">I provider di configurazione seguenti sono dedicati per specifici tipi di file:</span><span class="sxs-lookup"><span data-stu-id="1c59b-367">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="1c59b-368">Provider di configurazione INI</span><span class="sxs-lookup"><span data-stu-id="1c59b-368">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="1c59b-369">Provider di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="1c59b-369">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="1c59b-370">Provider di configurazione XML</span><span class="sxs-lookup"><span data-stu-id="1c59b-370">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="1c59b-371">Provider di configurazione INI</span><span class="sxs-lookup"><span data-stu-id="1c59b-371">INI Configuration Provider</span></span>

<span data-ttu-id="1c59b-372"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> carica la configurazione da coppie chiave-valore di file INI in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-372">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="1c59b-373">Per attivare la configurazione di file INI, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1c59b-373">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1c59b-374">I due punti possono essere usati come delimitatore di sezione nella configurazione di file INI.</span><span class="sxs-lookup"><span data-stu-id="1c59b-374">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="1c59b-375">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="1c59b-375">Overloads permit specifying:</span></span>

* <span data-ttu-id="1c59b-376">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="1c59b-376">Whether the file is optional.</span></span>
* <span data-ttu-id="1c59b-377">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="1c59b-377">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="1c59b-378">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="1c59b-378">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="1c59b-379">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="1c59b-379">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="1c59b-380">Un esempio generico di un file di configurazione INI:</span><span class="sxs-lookup"><span data-stu-id="1c59b-380">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="1c59b-381">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="1c59b-381">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="1c59b-382">section0:key0</span><span class="sxs-lookup"><span data-stu-id="1c59b-382">section0:key0</span></span>
* <span data-ttu-id="1c59b-383">section0:key1</span><span class="sxs-lookup"><span data-stu-id="1c59b-383">section0:key1</span></span>
* <span data-ttu-id="1c59b-384">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="1c59b-384">section1:subsection:key</span></span>
* <span data-ttu-id="1c59b-385">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="1c59b-385">section2:subsection0:key</span></span>
* <span data-ttu-id="1c59b-386">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="1c59b-386">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="1c59b-387">Provider di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="1c59b-387">JSON Configuration Provider</span></span>

<span data-ttu-id="1c59b-388">Il <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> carica la configurazione da coppie chiave-valore di file JSON in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-388">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="1c59b-389">Per attivare la configurazione di file JSON, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1c59b-389">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1c59b-390">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="1c59b-390">Overloads permit specifying:</span></span>

* <span data-ttu-id="1c59b-391">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="1c59b-391">Whether the file is optional.</span></span>
* <span data-ttu-id="1c59b-392">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="1c59b-392">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="1c59b-393">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="1c59b-393">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="1c59b-394">`AddJsonFile` viene chiamato automaticamente due volte quando si inizializza un nuovo generatore di host con `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-394">`AddJsonFile` is automatically called twice when you initialize a new host builder with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="1c59b-395">Il metodo viene chiamato per caricare la configurazione da:</span><span class="sxs-lookup"><span data-stu-id="1c59b-395">The method is called to load configuration from:</span></span>

* <span data-ttu-id="1c59b-396">*appsettings.json* &ndash; Questo file viene letto per primo.</span><span class="sxs-lookup"><span data-stu-id="1c59b-396">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="1c59b-397">La versione dell'ambiente del file può sostituire i valori forniti dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1c59b-397">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="1c59b-398">*appsettings.{Environment}.json* &ndash; La versione dell'ambiente del file viene caricata in base a [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="1c59b-398">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="1c59b-399">Per altre informazioni, vedere la sezione [Configurazione predefinita](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="1c59b-399">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="1c59b-400">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="1c59b-400">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="1c59b-401">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="1c59b-401">Environment variables.</span></span>
* <span data-ttu-id="1c59b-402">[Segreti utente (Secret Manager)](xref:security/app-secrets) nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="1c59b-402">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="1c59b-403">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="1c59b-403">Command-line arguments.</span></span>

<span data-ttu-id="1c59b-404">Il provider di configurazione JSON viene stabilito per primo.</span><span class="sxs-lookup"><span data-stu-id="1c59b-404">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="1c59b-405">I segreti utente, le variabili di ambiente e gli argomenti della riga di comando sostituiscono quindi la configurazione impostata dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="1c59b-405">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="1c59b-406">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare la configurazione dell'app per file diversi da *appsettings.json* e *appsettings.{Environment}.json*:</span><span class="sxs-lookup"><span data-stu-id="1c59b-406">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="1c59b-407">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="1c59b-407">**Example**</span></span>

<span data-ttu-id="1c59b-408">L'app di esempio consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include due chiamate a `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-408">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="1c59b-409">La configurazione viene caricata da *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="1c59b-409">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

1. <span data-ttu-id="1c59b-410">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="1c59b-410">Run the sample app.</span></span> <span data-ttu-id="1c59b-411">Aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-411">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="1c59b-412">Notare che l'output contiene coppie chiave-valore per la configurazione mostrata nella tabella in base all'ambiente.</span><span class="sxs-lookup"><span data-stu-id="1c59b-412">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="1c59b-413">Le chiavi di configurazione di registrazione usano i due punti (`:`) come separatore gerarchico.</span><span class="sxs-lookup"><span data-stu-id="1c59b-413">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="1c59b-414">Chiave</span><span class="sxs-lookup"><span data-stu-id="1c59b-414">Key</span></span>                        | <span data-ttu-id="1c59b-415">Valore Development</span><span class="sxs-lookup"><span data-stu-id="1c59b-415">Development Value</span></span> | <span data-ttu-id="1c59b-416">Valore Production</span><span class="sxs-lookup"><span data-stu-id="1c59b-416">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="1c59b-417">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="1c59b-417">Logging:LogLevel:System</span></span>    | <span data-ttu-id="1c59b-418">Informazioni</span><span class="sxs-lookup"><span data-stu-id="1c59b-418">Information</span></span>       | <span data-ttu-id="1c59b-419">Informazioni</span><span class="sxs-lookup"><span data-stu-id="1c59b-419">Information</span></span>      |
| <span data-ttu-id="1c59b-420">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="1c59b-420">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="1c59b-421">Informazioni</span><span class="sxs-lookup"><span data-stu-id="1c59b-421">Information</span></span>       | <span data-ttu-id="1c59b-422">Informazioni</span><span class="sxs-lookup"><span data-stu-id="1c59b-422">Information</span></span>      |
| <span data-ttu-id="1c59b-423">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="1c59b-423">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="1c59b-424">Debug</span><span class="sxs-lookup"><span data-stu-id="1c59b-424">Debug</span></span>             | <span data-ttu-id="1c59b-425">Error</span><span class="sxs-lookup"><span data-stu-id="1c59b-425">Error</span></span>            |
| <span data-ttu-id="1c59b-426">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="1c59b-426">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="1c59b-427">Provider di configurazione XML</span><span class="sxs-lookup"><span data-stu-id="1c59b-427">XML Configuration Provider</span></span>

<span data-ttu-id="1c59b-428">Il <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> carica la configurazione da coppie chiave-valore di file XML in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-428">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="1c59b-429">Per attivare la configurazione di file XML, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1c59b-429">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1c59b-430">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="1c59b-430">Overloads permit specifying:</span></span>

* <span data-ttu-id="1c59b-431">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="1c59b-431">Whether the file is optional.</span></span>
* <span data-ttu-id="1c59b-432">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="1c59b-432">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="1c59b-433">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="1c59b-433">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="1c59b-434">Il nodo radice del file di configurazione viene ignorato quando vengono create le coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-434">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="1c59b-435">Non specificare una definizione DTD (Document Type Definition) o uno spazio dei nomi nel file.</span><span class="sxs-lookup"><span data-stu-id="1c59b-435">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="1c59b-436">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="1c59b-436">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="1c59b-437">I file di configurazione XML possono usare nomi di elementi distinti per le sezioni ripetute:</span><span class="sxs-lookup"><span data-stu-id="1c59b-437">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="1c59b-438">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="1c59b-438">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="1c59b-439">section0:key0</span><span class="sxs-lookup"><span data-stu-id="1c59b-439">section0:key0</span></span>
* <span data-ttu-id="1c59b-440">section0:key1</span><span class="sxs-lookup"><span data-stu-id="1c59b-440">section0:key1</span></span>
* <span data-ttu-id="1c59b-441">section1:key0</span><span class="sxs-lookup"><span data-stu-id="1c59b-441">section1:key0</span></span>
* <span data-ttu-id="1c59b-442">section1:key1</span><span class="sxs-lookup"><span data-stu-id="1c59b-442">section1:key1</span></span>

<span data-ttu-id="1c59b-443">La ripetizione di elementi che usano lo stesso nome di elemento funziona se si usa l'attributo `name` per distinguere gli elementi:</span><span class="sxs-lookup"><span data-stu-id="1c59b-443">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="1c59b-444">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="1c59b-444">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="1c59b-445">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="1c59b-445">section:section0:key:key0</span></span>
* <span data-ttu-id="1c59b-446">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="1c59b-446">section:section0:key:key1</span></span>
* <span data-ttu-id="1c59b-447">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="1c59b-447">section:section1:key:key0</span></span>
* <span data-ttu-id="1c59b-448">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="1c59b-448">section:section1:key:key1</span></span>

<span data-ttu-id="1c59b-449">È possibile usare attributi per fornire valori:</span><span class="sxs-lookup"><span data-stu-id="1c59b-449">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="1c59b-450">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="1c59b-450">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="1c59b-451">key:attribute</span><span class="sxs-lookup"><span data-stu-id="1c59b-451">key:attribute</span></span>
* <span data-ttu-id="1c59b-452">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="1c59b-452">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="1c59b-453">Provider di configurazione KeyPerFile</span><span class="sxs-lookup"><span data-stu-id="1c59b-453">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="1c59b-454">Il <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> usa i file di una directory come coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-454">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="1c59b-455">La chiave è il nome del file.</span><span class="sxs-lookup"><span data-stu-id="1c59b-455">The key is the file name.</span></span> <span data-ttu-id="1c59b-456">Il valore contiene il contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="1c59b-456">The value contains the file's contents.</span></span> <span data-ttu-id="1c59b-457">Il provider di configurazione KeyPerFile viene usato in scenari di hosting Docker.</span><span class="sxs-lookup"><span data-stu-id="1c59b-457">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="1c59b-458">Per attivare la configurazione chiave-per-file, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1c59b-458">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="1c59b-459">Il `directoryPath` per i file deve essere un percorso assoluto.</span><span class="sxs-lookup"><span data-stu-id="1c59b-459">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="1c59b-460">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="1c59b-460">Overloads permit specifying:</span></span>

* <span data-ttu-id="1c59b-461">Un delegato `Action<KeyPerFileConfigurationSource>` che configura l'origine.</span><span class="sxs-lookup"><span data-stu-id="1c59b-461">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="1c59b-462">Se la directory è facoltativa e il percorso della directory.</span><span class="sxs-lookup"><span data-stu-id="1c59b-462">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="1c59b-463">Il doppio carattere di sottolineatura (`__`) viene usato come delimitatore per le chiavi di configurazione nei nomi dei file.</span><span class="sxs-lookup"><span data-stu-id="1c59b-463">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="1c59b-464">Ad esempio, il nome di file `Logging__LogLevel__System` produce la chiave di configurazione `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-464">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="1c59b-465">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="1c59b-465">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="1c59b-466">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="1c59b-466">Memory Configuration Provider</span></span>

<span data-ttu-id="1c59b-467">Il <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> usa una raccolta in memoria come coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-467">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="1c59b-468">Per attivare la configurazione della raccolta in memoria, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1c59b-468">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1c59b-469">Il provider di configurazione può essere inizializzato con un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-469">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="1c59b-470">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare la configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="1c59b-470">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="1c59b-471">Nell'esempio seguente viene creato un dizionario di configurazione:</span><span class="sxs-lookup"><span data-stu-id="1c59b-471">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="1c59b-472">Il dizionario viene usato con una chiamata a `AddInMemoryCollection` per fornire la configurazione:</span><span class="sxs-lookup"><span data-stu-id="1c59b-472">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="1c59b-473">GetValue</span><span class="sxs-lookup"><span data-stu-id="1c59b-473">GetValue</span></span>

<span data-ttu-id="1c59b-474">[ConfigurationBinder. GetValue \<T >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) estrae un singolo valore dalla configurazione con una chiave specificata e lo converte nel tipo non di raccolta specificato.</span><span class="sxs-lookup"><span data-stu-id="1c59b-474">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="1c59b-475">Un overload accetta un valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="1c59b-475">An overload accepts a default value.</span></span>

<span data-ttu-id="1c59b-476">L'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="1c59b-476">The following example:</span></span>

* <span data-ttu-id="1c59b-477">Estrae il valore di stringa dalla configurazione con la chiave `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-477">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="1c59b-478">Se `NumberKey` non viene trovato nelle chiavi di configurazione, viene usato il valore predefinito di `99`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-478">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="1c59b-479">Assegna al valore il tipo `int`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-479">Types the value as an `int`.</span></span>
* <span data-ttu-id="1c59b-480">Memorizza il valore nella proprietà `NumberConfig` per l'uso da parte della pagina.</span><span class="sxs-lookup"><span data-stu-id="1c59b-480">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="1c59b-481">GetSection, GetChildren ed Exists</span><span class="sxs-lookup"><span data-stu-id="1c59b-481">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="1c59b-482">Per gli esempi che seguono, prendere in considerazione il file JSON seguente.</span><span class="sxs-lookup"><span data-stu-id="1c59b-482">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="1c59b-483">Vengono trovate quattro chiavi in due sezioni, una delle quali include una coppia di sottosezioni:</span><span class="sxs-lookup"><span data-stu-id="1c59b-483">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="1c59b-484">Quando il file viene letto nella configurazione, vengono create le chiavi gerarchiche univoche seguenti per contenere i valori di configurazione:</span><span class="sxs-lookup"><span data-stu-id="1c59b-484">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="1c59b-485">section0:key0</span><span class="sxs-lookup"><span data-stu-id="1c59b-485">section0:key0</span></span>
* <span data-ttu-id="1c59b-486">section0:key1</span><span class="sxs-lookup"><span data-stu-id="1c59b-486">section0:key1</span></span>
* <span data-ttu-id="1c59b-487">section1:key0</span><span class="sxs-lookup"><span data-stu-id="1c59b-487">section1:key0</span></span>
* <span data-ttu-id="1c59b-488">section1:key1</span><span class="sxs-lookup"><span data-stu-id="1c59b-488">section1:key1</span></span>
* <span data-ttu-id="1c59b-489">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="1c59b-489">section2:subsection0:key0</span></span>
* <span data-ttu-id="1c59b-490">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="1c59b-490">section2:subsection0:key1</span></span>
* <span data-ttu-id="1c59b-491">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="1c59b-491">section2:subsection1:key0</span></span>
* <span data-ttu-id="1c59b-492">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="1c59b-492">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="1c59b-493">GetSection</span><span class="sxs-lookup"><span data-stu-id="1c59b-493">GetSection</span></span>

<span data-ttu-id="1c59b-494">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) estrae una sottosezione della configurazione con la chiave di sottosezione specificata.</span><span class="sxs-lookup"><span data-stu-id="1c59b-494">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="1c59b-495">Per restituire una <xref:Microsoft.Extensions.Configuration.IConfigurationSection> che contiene solo le coppie chiave-valore in `section1`, chiamare `GetSection` e fornire il nome della sezione:</span><span class="sxs-lookup"><span data-stu-id="1c59b-495">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="1c59b-496">`configSection` non ha un valore, ma solo una chiave e un percorso.</span><span class="sxs-lookup"><span data-stu-id="1c59b-496">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="1c59b-497">In modo analogo, per ottenere i valori per le chiavi in `section2:subsection0`, chiamare `GetSection` e fornire il percorso della sezione:</span><span class="sxs-lookup"><span data-stu-id="1c59b-497">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="1c59b-498">`GetSection` non restituisce mai `null`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-498">`GetSection` never returns `null`.</span></span> <span data-ttu-id="1c59b-499">Se non viene trovata una sezione corrispondente, viene restituita una `IConfigurationSection` vuota.</span><span class="sxs-lookup"><span data-stu-id="1c59b-499">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="1c59b-500">Quando `GetSection` restituisce una sezione corrispondente, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> non viene compilata.</span><span class="sxs-lookup"><span data-stu-id="1c59b-500">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="1c59b-501">Quando la sezione esiste, vengono restituiti <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> e <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path>.</span><span class="sxs-lookup"><span data-stu-id="1c59b-501">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="1c59b-502">GetChildren</span><span class="sxs-lookup"><span data-stu-id="1c59b-502">GetChildren</span></span>

<span data-ttu-id="1c59b-503">Una chiamata a [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) per `section2` ottiene una `IEnumerable<IConfigurationSection>` che include:</span><span class="sxs-lookup"><span data-stu-id="1c59b-503">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="1c59b-504">Esistente</span><span class="sxs-lookup"><span data-stu-id="1c59b-504">Exists</span></span>

<span data-ttu-id="1c59b-505">Usare [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) per determinare se esiste una sezione di configurazione:</span><span class="sxs-lookup"><span data-stu-id="1c59b-505">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="1c59b-506">Considerati i dati di esempio, `sectionExists` è `false` perché non esiste una sezione `section2:subsection2` nei dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-506">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="1c59b-507">Eseguire l'associazione a una classe</span><span class="sxs-lookup"><span data-stu-id="1c59b-507">Bind to a class</span></span>

<span data-ttu-id="1c59b-508">La configurazione può essere associata alle classi che rappresentano gruppi di impostazioni correlate usando il *modello di opzioni*.</span><span class="sxs-lookup"><span data-stu-id="1c59b-508">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="1c59b-509">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="1c59b-509">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="1c59b-510">I valori di configurazione vengono restituiti come stringhe, ma la chiamata di <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> consente la costruzione di oggetti [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="1c59b-510">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="1c59b-511">L'app di esempio contiene un modello `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="1c59b-511">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1c59b-512">La sezione `starship` del file *starship.json* crea la configurazione quando l'app di esempio usa il provider di configurazione JSON per caricare la configurazione:</span><span class="sxs-lookup"><span data-stu-id="1c59b-512">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="1c59b-513">Vengono create le coppie chiave-valore della configurazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="1c59b-513">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="1c59b-514">Chiave</span><span class="sxs-lookup"><span data-stu-id="1c59b-514">Key</span></span>                   | <span data-ttu-id="1c59b-515">Value</span><span class="sxs-lookup"><span data-stu-id="1c59b-515">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="1c59b-516">starship:name</span><span class="sxs-lookup"><span data-stu-id="1c59b-516">starship:name</span></span>         | <span data-ttu-id="1c59b-517">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="1c59b-517">USS Enterprise</span></span>                                    |
| <span data-ttu-id="1c59b-518">starship:registry</span><span class="sxs-lookup"><span data-stu-id="1c59b-518">starship:registry</span></span>     | <span data-ttu-id="1c59b-519">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="1c59b-519">NCC-1701</span></span>                                          |
| <span data-ttu-id="1c59b-520">starship:class</span><span class="sxs-lookup"><span data-stu-id="1c59b-520">starship:class</span></span>        | <span data-ttu-id="1c59b-521">Constitution</span><span class="sxs-lookup"><span data-stu-id="1c59b-521">Constitution</span></span>                                      |
| <span data-ttu-id="1c59b-522">starship:length</span><span class="sxs-lookup"><span data-stu-id="1c59b-522">starship:length</span></span>       | <span data-ttu-id="1c59b-523">304.8</span><span class="sxs-lookup"><span data-stu-id="1c59b-523">304.8</span></span>                                             |
| <span data-ttu-id="1c59b-524">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="1c59b-524">starship:commissioned</span></span> | <span data-ttu-id="1c59b-525">False</span><span class="sxs-lookup"><span data-stu-id="1c59b-525">False</span></span>                                             |
| <span data-ttu-id="1c59b-526">trademark</span><span class="sxs-lookup"><span data-stu-id="1c59b-526">trademark</span></span>             | <span data-ttu-id="1c59b-527">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="1c59b-527">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="1c59b-528">L'app di esempio chiama `GetSection` con la chiave `starship`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-528">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="1c59b-529">Le coppie chiave-valore `starship` sono isolate.</span><span class="sxs-lookup"><span data-stu-id="1c59b-529">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="1c59b-530">Il metodo `Bind` viene chiamato sulla sottosezione passando un'istanza della classe `Starship`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-530">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="1c59b-531">Dopo aver associato i valori dell'istanza, l'istanza viene assegnata a una proprietà per il rendering:</span><span class="sxs-lookup"><span data-stu-id="1c59b-531">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="1c59b-532">Associazione a un oggetto grafico</span><span class="sxs-lookup"><span data-stu-id="1c59b-532">Bind to an object graph</span></span>

<span data-ttu-id="1c59b-533"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> è in grado di associare un intero oggetto grafico POCO.</span><span class="sxs-lookup"><span data-stu-id="1c59b-533"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="1c59b-534">L'esempio contiene un modello `TvShow` il cui oggetto grafico include `Metadata` e le classi `Actors` (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="1c59b-534">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1c59b-535">L'app di esempio ha un file *tvshow.xml* che contiene i dati di configurazione:</span><span class="sxs-lookup"><span data-stu-id="1c59b-535">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="1c59b-536">La configurazione viene associata all'intero oggetto grafico `TvShow` con il metodo `Bind`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-536">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="1c59b-537">L'istanza associata viene assegnata a una proprietà per il rendering:</span><span class="sxs-lookup"><span data-stu-id="1c59b-537">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="1c59b-538">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) associa e restituisce il tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="1c59b-538">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="1c59b-539">`Get<T>` può essere più comodo che usare `Bind`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-539">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="1c59b-540">Il codice seguente mostra come usare `Get<T>` con l'esempio precedente, che consente di assegnare l'istanza associata direttamente alla proprietà usata per il rendering:</span><span class="sxs-lookup"><span data-stu-id="1c59b-540">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="1c59b-541">Associare una matrice a una classe</span><span class="sxs-lookup"><span data-stu-id="1c59b-541">Bind an array to a class</span></span>

<span data-ttu-id="1c59b-542">*L'app di esempio dimostra i concetti spiegati in questa sezione.*</span><span class="sxs-lookup"><span data-stu-id="1c59b-542">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="1c59b-543">Il <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supporta l'associazione di matrici agli oggetti usando gli indici delle matrici nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-543">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="1c59b-544">Qualsiasi formato di matrice che espone un segmento chiave numerico (`:0:`, `:1:`, &hellip; `:{n}:`) supporta l'associazione di matrici a una matrice di classi POCO.</span><span class="sxs-lookup"><span data-stu-id="1c59b-544">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="1c59b-545">L'associazione viene fornita per convenzione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-545">Binding is provided by convention.</span></span> <span data-ttu-id="1c59b-546">I provider di configurazione personalizzati non devono implementare l'associazione di matrici.</span><span class="sxs-lookup"><span data-stu-id="1c59b-546">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="1c59b-547">**Elaborazione di matrici in memoria**</span><span class="sxs-lookup"><span data-stu-id="1c59b-547">**In-memory array processing**</span></span>

<span data-ttu-id="1c59b-548">Prendere in considerazione le chiavi di configurazione e i valori indicati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="1c59b-548">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="1c59b-549">Chiave</span><span class="sxs-lookup"><span data-stu-id="1c59b-549">Key</span></span>             | <span data-ttu-id="1c59b-550">Value</span><span class="sxs-lookup"><span data-stu-id="1c59b-550">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="1c59b-551">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="1c59b-551">array:entries:0</span></span> | <span data-ttu-id="1c59b-552">value0</span><span class="sxs-lookup"><span data-stu-id="1c59b-552">value0</span></span> |
| <span data-ttu-id="1c59b-553">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="1c59b-553">array:entries:1</span></span> | <span data-ttu-id="1c59b-554">value1</span><span class="sxs-lookup"><span data-stu-id="1c59b-554">value1</span></span> |
| <span data-ttu-id="1c59b-555">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="1c59b-555">array:entries:2</span></span> | <span data-ttu-id="1c59b-556">value2</span><span class="sxs-lookup"><span data-stu-id="1c59b-556">value2</span></span> |
| <span data-ttu-id="1c59b-557">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="1c59b-557">array:entries:4</span></span> | <span data-ttu-id="1c59b-558">value4</span><span class="sxs-lookup"><span data-stu-id="1c59b-558">value4</span></span> |
| <span data-ttu-id="1c59b-559">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="1c59b-559">array:entries:5</span></span> | <span data-ttu-id="1c59b-560">value5</span><span class="sxs-lookup"><span data-stu-id="1c59b-560">value5</span></span> |

<span data-ttu-id="1c59b-561">Queste chiavi e i valori vengono caricati nell'app di esempio usando il provider di configurazione della memoria:</span><span class="sxs-lookup"><span data-stu-id="1c59b-561">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

<span data-ttu-id="1c59b-562">La matrice ignora un valore per l'indice &num;3.</span><span class="sxs-lookup"><span data-stu-id="1c59b-562">The array skips a value for index &num;3.</span></span> <span data-ttu-id="1c59b-563">Lo strumento di associazione di configurazione non è in grado di associare valori null o di creare voci null negli oggetti associati, situazione che diventerà chiara tra poco quando viene illustrato il risultato dell'associazione di questa matrice a un oggetto.</span><span class="sxs-lookup"><span data-stu-id="1c59b-563">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="1c59b-564">Nell'app di esempio, è disponibile una classe POCO per i dati di configurazione associati:</span><span class="sxs-lookup"><span data-stu-id="1c59b-564">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1c59b-565">I dati di configurazione sono associati all'oggetto:</span><span class="sxs-lookup"><span data-stu-id="1c59b-565">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="1c59b-566">È possibile usare anche la sintassi [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*), che produce codice più compatto:</span><span class="sxs-lookup"><span data-stu-id="1c59b-566">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="1c59b-567">L'oggetto associato, un'istanza di `ArrayExample`, riceve i dati di matrice dalla configurazione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-567">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="1c59b-568">Indice di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="1c59b-568">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="1c59b-569">Valore di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="1c59b-569">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="1c59b-570">0</span><span class="sxs-lookup"><span data-stu-id="1c59b-570">0</span></span>                            | <span data-ttu-id="1c59b-571">value0</span><span class="sxs-lookup"><span data-stu-id="1c59b-571">value0</span></span>                       |
| <span data-ttu-id="1c59b-572">1</span><span class="sxs-lookup"><span data-stu-id="1c59b-572">1</span></span>                            | <span data-ttu-id="1c59b-573">value1</span><span class="sxs-lookup"><span data-stu-id="1c59b-573">value1</span></span>                       |
| <span data-ttu-id="1c59b-574">2</span><span class="sxs-lookup"><span data-stu-id="1c59b-574">2</span></span>                            | <span data-ttu-id="1c59b-575">value2</span><span class="sxs-lookup"><span data-stu-id="1c59b-575">value2</span></span>                       |
| <span data-ttu-id="1c59b-576">3\.</span><span class="sxs-lookup"><span data-stu-id="1c59b-576">3</span></span>                            | <span data-ttu-id="1c59b-577">value4</span><span class="sxs-lookup"><span data-stu-id="1c59b-577">value4</span></span>                       |
| <span data-ttu-id="1c59b-578">4</span><span class="sxs-lookup"><span data-stu-id="1c59b-578">4</span></span>                            | <span data-ttu-id="1c59b-579">value5</span><span class="sxs-lookup"><span data-stu-id="1c59b-579">value5</span></span>                       |

<span data-ttu-id="1c59b-580">L'indice &num;3 nell'oggetto associato contiene i dati di configurazione per la chiave di configurazione `array:4` e il relativo valore `value4`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-580">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="1c59b-581">Quando i dati di configurazione contenenti una matrice vengono associati, gli indici di matrice nelle chiavi di configurazione vengono usati semplicemente per scorrere i dati di configurazione quando si crea l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="1c59b-581">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="1c59b-582">Un valore null non può essere mantenuto nei dati di configurazione e una voce con valore null non viene creata in un oggetto associato quando una matrice nelle chiavi di configurazione ignora uno o più indici.</span><span class="sxs-lookup"><span data-stu-id="1c59b-582">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="1c59b-583">L'elemento di configurazione mancante per l'indice &num;3 può essere fornito prima dell'associazione all'istanza `ArrayExample` da qualsiasi provider di configurazione che produce la coppia chiave-valore corretta nella configurazione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-583">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="1c59b-584">Se il codice di esempio include un provider di configurazione JSON aggiuntivo con la coppia chiave-valore mancante, `ArrayExample.Entries` corrisponde alla matrice di configurazione completa:</span><span class="sxs-lookup"><span data-stu-id="1c59b-584">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="1c59b-585">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="1c59b-585">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="1c59b-586">In `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="1c59b-586">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="1c59b-587">La coppia chiave-valore mostrata nella tabella viene caricata nella configurazione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-587">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="1c59b-588">Chiave</span><span class="sxs-lookup"><span data-stu-id="1c59b-588">Key</span></span>             | <span data-ttu-id="1c59b-589">Value</span><span class="sxs-lookup"><span data-stu-id="1c59b-589">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="1c59b-590">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="1c59b-590">array:entries:3</span></span> | <span data-ttu-id="1c59b-591">value3</span><span class="sxs-lookup"><span data-stu-id="1c59b-591">value3</span></span> |

<span data-ttu-id="1c59b-592">Se l'istanza della classe `ArrayExample` viene associata dopo che il provider di configurazione JSON include la voce per l'indice &num;3, la matrice `ArrayExample.Entries` include il valore.</span><span class="sxs-lookup"><span data-stu-id="1c59b-592">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="1c59b-593">Indice di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="1c59b-593">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="1c59b-594">Valore di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="1c59b-594">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="1c59b-595">0</span><span class="sxs-lookup"><span data-stu-id="1c59b-595">0</span></span>                            | <span data-ttu-id="1c59b-596">value0</span><span class="sxs-lookup"><span data-stu-id="1c59b-596">value0</span></span>                       |
| <span data-ttu-id="1c59b-597">1</span><span class="sxs-lookup"><span data-stu-id="1c59b-597">1</span></span>                            | <span data-ttu-id="1c59b-598">value1</span><span class="sxs-lookup"><span data-stu-id="1c59b-598">value1</span></span>                       |
| <span data-ttu-id="1c59b-599">2</span><span class="sxs-lookup"><span data-stu-id="1c59b-599">2</span></span>                            | <span data-ttu-id="1c59b-600">value2</span><span class="sxs-lookup"><span data-stu-id="1c59b-600">value2</span></span>                       |
| <span data-ttu-id="1c59b-601">3\.</span><span class="sxs-lookup"><span data-stu-id="1c59b-601">3</span></span>                            | <span data-ttu-id="1c59b-602">value3</span><span class="sxs-lookup"><span data-stu-id="1c59b-602">value3</span></span>                       |
| <span data-ttu-id="1c59b-603">4</span><span class="sxs-lookup"><span data-stu-id="1c59b-603">4</span></span>                            | <span data-ttu-id="1c59b-604">value4</span><span class="sxs-lookup"><span data-stu-id="1c59b-604">value4</span></span>                       |
| <span data-ttu-id="1c59b-605">5</span><span class="sxs-lookup"><span data-stu-id="1c59b-605">5</span></span>                            | <span data-ttu-id="1c59b-606">value5</span><span class="sxs-lookup"><span data-stu-id="1c59b-606">value5</span></span>                       |

<span data-ttu-id="1c59b-607">**Elaborazione di matrice JSON**</span><span class="sxs-lookup"><span data-stu-id="1c59b-607">**JSON array processing**</span></span>

<span data-ttu-id="1c59b-608">Se un file JSON contiene una matrice, vengono create chiavi di configurazione per gli elementi della matrice con un indice di sezione in base zero.</span><span class="sxs-lookup"><span data-stu-id="1c59b-608">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="1c59b-609">Nel file di configurazione seguente, `subsection` è una matrice:</span><span class="sxs-lookup"><span data-stu-id="1c59b-609">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="1c59b-610">Il provider di configurazione JSON legge i dati di configurazione nelle coppie chiave-valore seguenti:</span><span class="sxs-lookup"><span data-stu-id="1c59b-610">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="1c59b-611">Chiave</span><span class="sxs-lookup"><span data-stu-id="1c59b-611">Key</span></span>                     | <span data-ttu-id="1c59b-612">Value</span><span class="sxs-lookup"><span data-stu-id="1c59b-612">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="1c59b-613">json_array:key</span><span class="sxs-lookup"><span data-stu-id="1c59b-613">json_array:key</span></span>          | <span data-ttu-id="1c59b-614">valueA</span><span class="sxs-lookup"><span data-stu-id="1c59b-614">valueA</span></span> |
| <span data-ttu-id="1c59b-615">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="1c59b-615">json_array:subsection:0</span></span> | <span data-ttu-id="1c59b-616">valueB</span><span class="sxs-lookup"><span data-stu-id="1c59b-616">valueB</span></span> |
| <span data-ttu-id="1c59b-617">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="1c59b-617">json_array:subsection:1</span></span> | <span data-ttu-id="1c59b-618">valueC</span><span class="sxs-lookup"><span data-stu-id="1c59b-618">valueC</span></span> |
| <span data-ttu-id="1c59b-619">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="1c59b-619">json_array:subsection:2</span></span> | <span data-ttu-id="1c59b-620">valueD</span><span class="sxs-lookup"><span data-stu-id="1c59b-620">valueD</span></span> |

<span data-ttu-id="1c59b-621">Nell'app di esempio, la classe POCO seguente è disponibile per associare le coppie chiave-valore della configurazione:</span><span class="sxs-lookup"><span data-stu-id="1c59b-621">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="1c59b-622">Dopo l'associazione, `JsonArrayExample.Key` contiene il valore `valueA`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-622">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="1c59b-623">I valori di sottosezione vengono archiviati nella proprietà matrice POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-623">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="1c59b-624">Indice di `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="1c59b-624">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="1c59b-625">Valore di `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="1c59b-625">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="1c59b-626">0</span><span class="sxs-lookup"><span data-stu-id="1c59b-626">0</span></span>                                   | <span data-ttu-id="1c59b-627">valueB</span><span class="sxs-lookup"><span data-stu-id="1c59b-627">valueB</span></span>                              |
| <span data-ttu-id="1c59b-628">1</span><span class="sxs-lookup"><span data-stu-id="1c59b-628">1</span></span>                                   | <span data-ttu-id="1c59b-629">valueC</span><span class="sxs-lookup"><span data-stu-id="1c59b-629">valueC</span></span>                              |
| <span data-ttu-id="1c59b-630">2</span><span class="sxs-lookup"><span data-stu-id="1c59b-630">2</span></span>                                   | <span data-ttu-id="1c59b-631">valueD</span><span class="sxs-lookup"><span data-stu-id="1c59b-631">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="1c59b-632">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="1c59b-632">Custom configuration provider</span></span>

<span data-ttu-id="1c59b-633">L'app di esempio dimostra come creare un provider di configurazione di base che legge le coppie chiave-valore di configurazione da un database usando [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="1c59b-633">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="1c59b-634">Il provider ha le caratteristiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="1c59b-634">The provider has the following characteristics:</span></span>

* <span data-ttu-id="1c59b-635">Il database in memoria di Entity Framework viene usato a scopo dimostrativo.</span><span class="sxs-lookup"><span data-stu-id="1c59b-635">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="1c59b-636">Per usare un database che richiede una stringa di connessione, implementare un `ConfigurationBuilder` secondario per fornire la stringa di connessione da un altro provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-636">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="1c59b-637">Il provider legge una tabella di database in una configurazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="1c59b-637">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="1c59b-638">Il provider non esegue una query sul database per ogni chiave.</span><span class="sxs-lookup"><span data-stu-id="1c59b-638">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="1c59b-639">Il ricaricamento in caso di modifica non è implementato, quindi l'aggiornamento del database dopo l'avvio dell'app non ha alcun effetto sulla configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="1c59b-639">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="1c59b-640">Definire un'entità `EFConfigurationValue` per l'archiviazione dei valori di configurazione nel database.</span><span class="sxs-lookup"><span data-stu-id="1c59b-640">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1c59b-641">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c59b-641">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="1c59b-642">Aggiungere `EFConfigurationContext` per archiviare i valori configurati e accedervi.</span><span class="sxs-lookup"><span data-stu-id="1c59b-642">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="1c59b-643">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c59b-643">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="1c59b-644">Creare una classe che implementi <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="1c59b-644">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="1c59b-645">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c59b-645">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="1c59b-646">Creare il provider di configurazione personalizzato ereditando da <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="1c59b-646">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="1c59b-647">Il provider di configurazione inizializza il database quando è vuoto.</span><span class="sxs-lookup"><span data-stu-id="1c59b-647">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="1c59b-648">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c59b-648">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="1c59b-649">Un metodo di estensione `AddEFConfiguration` consente di aggiungere l'origine di configurazione a un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-649">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="1c59b-650">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c59b-650">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="1c59b-651">L'esempio di codice seguente mostra come usare il `EFConfigurationProvider` personalizzato in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c59b-651">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1c59b-652">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c59b-652">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="1c59b-653">Aggiungere `EFConfigurationContext` per archiviare i valori configurati e accedervi.</span><span class="sxs-lookup"><span data-stu-id="1c59b-653">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="1c59b-654">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c59b-654">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="1c59b-655">Creare una classe che implementi <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="1c59b-655">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="1c59b-656">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c59b-656">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="1c59b-657">Creare il provider di configurazione personalizzato ereditando da <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="1c59b-657">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="1c59b-658">Il provider di configurazione inizializza il database quando è vuoto.</span><span class="sxs-lookup"><span data-stu-id="1c59b-658">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="1c59b-659">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c59b-659">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="1c59b-660">Un metodo di estensione `AddEFConfiguration` consente di aggiungere l'origine di configurazione a un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-660">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="1c59b-661">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c59b-661">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="1c59b-662">L'esempio di codice seguente mostra come usare il `EFConfigurationProvider` personalizzato in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="1c59b-662">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="1c59b-663">Accedere alla configurazione durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="1c59b-663">Access configuration during startup</span></span>

<span data-ttu-id="1c59b-664">Inserire `IConfiguration` nel costruttore `Startup` per accedere ai valori di configurazione in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1c59b-664">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1c59b-665">Per accedere alla configurazione in `Startup.Configure`, inserire `IConfiguration` direttamente nel metodo o usare l'istanza dal costruttore:</span><span class="sxs-lookup"><span data-stu-id="1c59b-665">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="1c59b-666">Per un esempio di accesso alla configurazione usando metodi di servizio di avvio, vedere [Avvio dell'applicazione: Metodi pratici](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="1c59b-666">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="1c59b-667">Accedere alla configurazione in una pagina Razor Pages o in una visualizzazione MVC</span><span class="sxs-lookup"><span data-stu-id="1c59b-667">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="1c59b-668">Per accedere alle impostazioni di configurazione in una pagina Razor Pages o in una visualizzazione MVC, aggiungere un [direttiva using](xref:mvc/views/razor#using) ([Informazioni di riferimento su C#: direttiva using](/dotnet/csharp/language-reference/keywords/using-directive)) per lo [spazio dei nomi Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) e inserire <xref:Microsoft.Extensions.Configuration.IConfiguration> nella pagina o nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="1c59b-668">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="1c59b-669">In una pagina Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="1c59b-669">In a Razor Pages page:</span></span>

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

<span data-ttu-id="1c59b-670">In una visualizzazione MVC:</span><span class="sxs-lookup"><span data-stu-id="1c59b-670">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="1c59b-671">Aggiungere la configurazione da un assembly esterno</span><span class="sxs-lookup"><span data-stu-id="1c59b-671">Add configuration from an external assembly</span></span>

<span data-ttu-id="1c59b-672">Un'implementazione <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> consente l'aggiunta di miglioramenti a un'app all'avvio, da un assembly esterno alla classe `Startup` dell'app.</span><span class="sxs-lookup"><span data-stu-id="1c59b-672">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="1c59b-673">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="1c59b-673">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1c59b-674">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="1c59b-674">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
