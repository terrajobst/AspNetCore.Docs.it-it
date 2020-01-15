---
title: Configurazione in ASP.NET Core
author: guardrex
description: Informazioni su come usare l'API di configurazione per configurare un'app ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: 09ef06f179e34cd7f4f04ac30c3b5dd95d058244
ms.sourcegitcommit: 2388c2a7334ce66b6be3ffbab06dd7923df18f60
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75951863"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="f1637-103">Configurazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f1637-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="f1637-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f1637-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f1637-105">La configurazione delle app in ASP.NET Core si basa su coppie chiave-valore stabilite dai *provider di configurazione*.</span><span class="sxs-lookup"><span data-stu-id="f1637-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="f1637-106">I provider di configurazione leggono i dati di configurazione in coppie chiave-valore da un'ampia gamma di origini di configurazione:</span><span class="sxs-lookup"><span data-stu-id="f1637-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="f1637-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="f1637-107">Azure Key Vault</span></span>
* <span data-ttu-id="f1637-108">Configurazione app di Azure</span><span class="sxs-lookup"><span data-stu-id="f1637-108">Azure App Configuration</span></span>
* <span data-ttu-id="f1637-109">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="f1637-109">Command-line arguments</span></span>
* <span data-ttu-id="f1637-110">Provider personalizzati (installati o creati)</span><span class="sxs-lookup"><span data-stu-id="f1637-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="f1637-111">File della directory</span><span class="sxs-lookup"><span data-stu-id="f1637-111">Directory files</span></span>
* <span data-ttu-id="f1637-112">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="f1637-112">Environment variables</span></span>
* <span data-ttu-id="f1637-113">Oggetti .NET in memoria</span><span class="sxs-lookup"><span data-stu-id="f1637-113">In-memory .NET objects</span></span>
* <span data-ttu-id="f1637-114">File di impostazioni</span><span class="sxs-lookup"><span data-stu-id="f1637-114">Settings files</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f1637-115">I pacchetti di configurazione per gli scenari di provider di configurazione comuni ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) sono inclusi in modo implicito dal framework.</span><span class="sxs-lookup"><span data-stu-id="f1637-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f1637-116">I pacchetti di configurazione per gli scenari di provider di configurazione comuni ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) sono inclusi nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="f1637-116">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="f1637-117">Gli esempi di codice che seguono e quelli inclusi nell'app di esempio usano lo spazio dei nomi <xref:Microsoft.Extensions.Configuration>:</span><span class="sxs-lookup"><span data-stu-id="f1637-117">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="f1637-118">Il *modello di opzioni* è un'estensione dei concetti di configurazione descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="f1637-118">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="f1637-119">Le opzioni usano le classi per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="f1637-119">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="f1637-120">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="f1637-120">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="f1637-121">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f1637-121">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="f1637-122">Host e configurazione delle app</span><span class="sxs-lookup"><span data-stu-id="f1637-122">Host versus app configuration</span></span>

<span data-ttu-id="f1637-123">Prima che l'app venga configurata e avviata, viene configurato e avviato un *host*.</span><span class="sxs-lookup"><span data-stu-id="f1637-123">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="f1637-124">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="f1637-124">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="f1637-125">Sia l'app che l'host vengono configurati tramite i provider di configurazione descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="f1637-125">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="f1637-126">Anche le coppie chiave-valore di configurazione dell'host sono incluse nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="f1637-126">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="f1637-127">Per altre informazioni su come vengono usati i provider di configurazione quando viene creato l'host e sugli effetti delle origini di configurazione sull'host di configurazione, vedere <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="f1637-127">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="f1637-128">Configurazione predefinita</span><span class="sxs-lookup"><span data-stu-id="f1637-128">Default configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f1637-129">Le app basate sui modelli [dotnet new](/dotnet/core/tools/dotnet-new) di ASP.NET Core chiamano <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> durante la compilazione di un host.</span><span class="sxs-lookup"><span data-stu-id="f1637-129">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="f1637-130">`CreateDefaultBuilder` fornisce la configurazione predefinita per l'app nell'ordine seguente:</span><span class="sxs-lookup"><span data-stu-id="f1637-130">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="f1637-131">La configurazione seguente si applica alle app che usano l'[host generico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="f1637-131">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="f1637-132">Per informazioni dettagliate sulla configurazione predefinita quando viene usato l'[host Web](xref:fundamentals/host/web-host), vedere la [versione di questo argomento per ASP.NET Core 2.2](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="f1637-132">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="f1637-133">La configurazione dell'host viene specificata da:</span><span class="sxs-lookup"><span data-stu-id="f1637-133">Host configuration is provided from:</span></span>
  * <span data-ttu-id="f1637-134">Variabili di ambiente con prefisso `DOTNET_` (ad esempio `DOTNET_ENVIRONMENT`) che usano il [provider di configurazione delle variabili di ambiente](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f1637-134">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="f1637-135">Il prefisso (`DOTNET_`) viene rimosso al caricamento delle coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="f1637-135">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="f1637-136">Argomenti della riga di comando che usano il [provider di configurazione della riga di comando](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f1637-136">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="f1637-137">La configurazione predefinita dell'host Web viene stabilita (`ConfigureWebHostDefaults`) nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="f1637-137">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="f1637-138">Kestrel viene usato come server Web e configurato con i provider di configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="f1637-138">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="f1637-139">Aggiungere il middleware di filtro host.</span><span class="sxs-lookup"><span data-stu-id="f1637-139">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="f1637-140">Aggiungere il middleware delle intestazioni inoltrate se la variabile di ambiente `ASPNETCORE_FORWARDEDHEADERS_ENABLED` è impostata su `true`.</span><span class="sxs-lookup"><span data-stu-id="f1637-140">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="f1637-141">Abilitare l'integrazione di IIS.</span><span class="sxs-lookup"><span data-stu-id="f1637-141">Enable IIS integration.</span></span>
* <span data-ttu-id="f1637-142">La configurazione dell'app viene fornita da:</span><span class="sxs-lookup"><span data-stu-id="f1637-142">App configuration is provided from:</span></span>
  * <span data-ttu-id="f1637-143">*appsettings.json* mediante il [provider di configurazione dei file](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f1637-143">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="f1637-144">*appsettings.{Ambiente}.json* mediante il [provider di configurazione dei file](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f1637-144">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="f1637-145">[Strumento di gestione dei segreti](xref:security/app-secrets) quando l'app viene eseguita nell'ambiente `Development` usando l'assembly di ingresso.</span><span class="sxs-lookup"><span data-stu-id="f1637-145">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="f1637-146">Variabili di ambiente che usano il [provider di configurazione delle variabili di ambiente](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f1637-146">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="f1637-147">Argomenti della riga di comando che usano il [provider di configurazione della riga di comando](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f1637-147">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f1637-148">Le app basate sui modelli [dotnet new](/dotnet/core/tools/dotnet-new) di ASP.NET Core chiamano <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> durante la compilazione di un host.</span><span class="sxs-lookup"><span data-stu-id="f1637-148">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="f1637-149">`CreateDefaultBuilder` fornisce la configurazione predefinita per l'app nell'ordine seguente:</span><span class="sxs-lookup"><span data-stu-id="f1637-149">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="f1637-150">La configurazione seguente si applica alle app che usano l'[host Web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="f1637-150">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="f1637-151">Per informazioni dettagliate sulla configurazione predefinita quando viene usato l'[host generico](xref:fundamentals/host/generic-host), vedere la [versione più recente di questo argomento](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="f1637-151">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="f1637-152">La configurazione dell'host viene specificata da:</span><span class="sxs-lookup"><span data-stu-id="f1637-152">Host configuration is provided from:</span></span>
  * <span data-ttu-id="f1637-153">Variabili di ambiente con prefisso `ASPNETCORE_` (ad esempio `ASPNETCORE_ENVIRONMENT`) che usano il [provider di configurazione delle variabili di ambiente](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f1637-153">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="f1637-154">Il prefisso (`ASPNETCORE_`) viene rimosso al caricamento delle coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="f1637-154">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="f1637-155">Argomenti della riga di comando che usano il [provider di configurazione della riga di comando](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f1637-155">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="f1637-156">La configurazione dell'app viene fornita da:</span><span class="sxs-lookup"><span data-stu-id="f1637-156">App configuration is provided from:</span></span>
  * <span data-ttu-id="f1637-157">*appsettings.json* mediante il [provider di configurazione dei file](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f1637-157">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="f1637-158">*appsettings.{Ambiente}.json* mediante il [provider di configurazione dei file](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f1637-158">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="f1637-159">[Strumento di gestione dei segreti](xref:security/app-secrets) quando l'app viene eseguita nell'ambiente `Development` usando l'assembly di ingresso.</span><span class="sxs-lookup"><span data-stu-id="f1637-159">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="f1637-160">Variabili di ambiente che usano il [provider di configurazione delle variabili di ambiente](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f1637-160">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="f1637-161">Argomenti della riga di comando che usano il [provider di configurazione della riga di comando](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f1637-161">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

## <a name="security"></a><span data-ttu-id="f1637-162">Sicurezza -</span><span class="sxs-lookup"><span data-stu-id="f1637-162">Security</span></span>

<span data-ttu-id="f1637-163">Per proteggere i dati di configurazione sensibili, adottare le pratiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="f1637-163">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="f1637-164">Non archiviare mai la password o altri dati sensibili nel codice del provider di configurazione o in file di configurazione di testo normale.</span><span class="sxs-lookup"><span data-stu-id="f1637-164">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="f1637-165">Non usare i segreti di produzione in ambienti di sviluppo o di test.</span><span class="sxs-lookup"><span data-stu-id="f1637-165">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="f1637-166">Specificare i segreti all'esterno del progetto in modo che non possano essere inavvertitamente inviati a un repository del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="f1637-166">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="f1637-167">Per altre informazioni, vedere i seguenti argomenti:</span><span class="sxs-lookup"><span data-stu-id="f1637-167">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="f1637-168"><xref:security/app-secrets> &ndash; include consigli sull'utilizzo delle variabili di ambiente per l'archiviazione di dati sensibili.</span><span class="sxs-lookup"><span data-stu-id="f1637-168"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="f1637-169">Secret Manager usa il provider di configurazione dei file per archiviare i segreti utente in un file JSON nel sistema locale.</span><span class="sxs-lookup"><span data-stu-id="f1637-169">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="f1637-170">Il provider di configurazione dei file è descritto più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="f1637-170">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="f1637-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) archivia in modo sicuro i segreti delle app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1637-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="f1637-172">Per ulteriori informazioni, vedere <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="f1637-172">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="f1637-173">Dati di configurazione gerarchici</span><span class="sxs-lookup"><span data-stu-id="f1637-173">Hierarchical configuration data</span></span>

<span data-ttu-id="f1637-174">L'API di configurazione è in grado di mantenere i dati di configurazione gerarchici rendendoli flat grazie all'uso di un delimitatore nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f1637-174">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="f1637-175">Nel file JSON seguente, esistono quattro chiavi in una gerarchia strutturata di due sezioni:</span><span class="sxs-lookup"><span data-stu-id="f1637-175">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="f1637-176">Quando il file viene letto nella configurazione, vengono create chiavi univoche per mantenere la struttura di dati gerarchici originale dell'origine di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f1637-176">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="f1637-177">Le sezioni e le chiavi vengono rese flat usando due punti (`:`) per mantenere la struttura originale:</span><span class="sxs-lookup"><span data-stu-id="f1637-177">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="f1637-178">section0:key0</span><span class="sxs-lookup"><span data-stu-id="f1637-178">section0:key0</span></span>
* <span data-ttu-id="f1637-179">section0:key1</span><span class="sxs-lookup"><span data-stu-id="f1637-179">section0:key1</span></span>
* <span data-ttu-id="f1637-180">section1:key0</span><span class="sxs-lookup"><span data-stu-id="f1637-180">section1:key0</span></span>
* <span data-ttu-id="f1637-181">section1:key1</span><span class="sxs-lookup"><span data-stu-id="f1637-181">section1:key1</span></span>

<span data-ttu-id="f1637-182">I metodi <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> e <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> sono disponibili per isolare le sezioni e gli elementi figlio di una sezione nei dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f1637-182"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="f1637-183">Questi metodi sono descritti più avanti in [GetSection, GetChildren ed Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="f1637-183">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="f1637-184">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="f1637-184">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="f1637-185">Origini e provider</span><span class="sxs-lookup"><span data-stu-id="f1637-185">Sources and providers</span></span>

<span data-ttu-id="f1637-186">All'avvio dell'app, le origini di configurazione vengono lette nell'ordine con cui sono specificati i rispettivi provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f1637-186">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="f1637-187">I provider di configurazione che implementano il rilevamento delle modifiche sono in grado di ricaricare la configurazione quando viene modificata un'impostazione sottostante.</span><span class="sxs-lookup"><span data-stu-id="f1637-187">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="f1637-188">Ad esempio, il provider di configurazione dei file (descritto più avanti in questo argomento) e il [provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration) implementano il rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="f1637-188">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="f1637-189"><xref:Microsoft.Extensions.Configuration.IConfiguration> è disponibile nel contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) dell'app.</span><span class="sxs-lookup"><span data-stu-id="f1637-189"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="f1637-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> possono essere inseriti in un Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> o MVC <xref:Microsoft.AspNetCore.Mvc.Controller> per ottenere la configurazione per la classe.</span><span class="sxs-lookup"><span data-stu-id="f1637-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="f1637-191">Negli esempi seguenti viene usato il campo `_config` per accedere ai valori di configurazione:</span><span class="sxs-lookup"><span data-stu-id="f1637-191">In the following examples, the `_config` field is used to access configuration values:</span></span>

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

<span data-ttu-id="f1637-192">I provider di configurazione non possono usare l'inserimento delle dipendenze, perché non è disponibile quando vengono configurati dall'host.</span><span class="sxs-lookup"><span data-stu-id="f1637-192">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="f1637-193">Keys</span><span class="sxs-lookup"><span data-stu-id="f1637-193">Keys</span></span>

<span data-ttu-id="f1637-194">Le chiavi di configurazione adottano le convenzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="f1637-194">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="f1637-195">Per le chiavi non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="f1637-195">Keys are case-insensitive.</span></span> <span data-ttu-id="f1637-196">Ad esempio, `ConnectionString` e `connectionstring` vengono considerate chiavi equivalenti.</span><span class="sxs-lookup"><span data-stu-id="f1637-196">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="f1637-197">Se un valore per la stessa chiave viene impostato dallo stesso provider di configurazione o da provider diversi, viene usato l'ultimo valore impostato per la chiave.</span><span class="sxs-lookup"><span data-stu-id="f1637-197">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="f1637-198">Chiavi gerarchiche</span><span class="sxs-lookup"><span data-stu-id="f1637-198">Hierarchical keys</span></span>
  * <span data-ttu-id="f1637-199">Nell'ambito dell'API di configurazione, il separatore due punti (`:`) funziona in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="f1637-199">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="f1637-200">Nelle variabili di ambiente, un separatore due punti potrebbe non funzionare in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="f1637-200">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="f1637-201">Il doppio carattere di sottolineatura (`__`) è supportato da tutte le piattaforme e viene convertito automaticamente nei due punti.</span><span class="sxs-lookup"><span data-stu-id="f1637-201">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="f1637-202">In Azure Key Vault, le chiavi gerarchiche usano `--` (due trattini) come separatore.</span><span class="sxs-lookup"><span data-stu-id="f1637-202">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="f1637-203">Scrivere il codice per sostituire i trattini con i due punti quando i segreti vengono caricati nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="f1637-203">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="f1637-204">Il <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supporta l'associazione di matrici agli oggetti usando gli indici delle matrici nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f1637-204">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="f1637-205">L'associazione di matrici è descritta nella sezione [Associare una matrice a una classe](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="f1637-205">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="f1637-206">Valori</span><span class="sxs-lookup"><span data-stu-id="f1637-206">Values</span></span>

<span data-ttu-id="f1637-207">I valori di configurazione adottano le convenzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="f1637-207">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="f1637-208">I valori sono stringhe.</span><span class="sxs-lookup"><span data-stu-id="f1637-208">Values are strings.</span></span>
* <span data-ttu-id="f1637-209">I valori null non possono essere archiviati nella configurazione o associati a oggetti.</span><span class="sxs-lookup"><span data-stu-id="f1637-209">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="f1637-210">Provider</span><span class="sxs-lookup"><span data-stu-id="f1637-210">Providers</span></span>

<span data-ttu-id="f1637-211">La tabella seguente mostra i provider di configurazione disponibili per le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f1637-211">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="f1637-212">Provider</span><span class="sxs-lookup"><span data-stu-id="f1637-212">Provider</span></span> | <span data-ttu-id="f1637-213">Fornisce la configurazione da&hellip;</span><span class="sxs-lookup"><span data-stu-id="f1637-213">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="f1637-214">[Provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="f1637-214">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="f1637-215">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="f1637-215">Azure Key Vault</span></span> |
| <span data-ttu-id="f1637-216">[Provider di Configurazione app](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Documentazione di Azure)</span><span class="sxs-lookup"><span data-stu-id="f1637-216">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="f1637-217">Configurazione app di Azure</span><span class="sxs-lookup"><span data-stu-id="f1637-217">Azure App Configuration</span></span> |
| [<span data-ttu-id="f1637-218">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="f1637-218">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="f1637-219">Parametri della riga di comando</span><span class="sxs-lookup"><span data-stu-id="f1637-219">Command-line parameters</span></span> |
| [<span data-ttu-id="f1637-220">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="f1637-220">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="f1637-221">Origine personalizzata</span><span class="sxs-lookup"><span data-stu-id="f1637-221">Custom source</span></span> |
| [<span data-ttu-id="f1637-222">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="f1637-222">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="f1637-223">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="f1637-223">Environment variables</span></span> |
| [<span data-ttu-id="f1637-224">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="f1637-224">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="f1637-225">File (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="f1637-225">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="f1637-226">Provider di configurazione chiave-per-file</span><span class="sxs-lookup"><span data-stu-id="f1637-226">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="f1637-227">File della directory</span><span class="sxs-lookup"><span data-stu-id="f1637-227">Directory files</span></span> |
| [<span data-ttu-id="f1637-228">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="f1637-228">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="f1637-229">Raccolte in memoria</span><span class="sxs-lookup"><span data-stu-id="f1637-229">In-memory collections</span></span> |
| <span data-ttu-id="f1637-230">[Segreti utente (Secret Manager)](xref:security/app-secrets) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="f1637-230">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="f1637-231">File nella directory dei profili utente</span><span class="sxs-lookup"><span data-stu-id="f1637-231">File in the user profile directory</span></span> |

<span data-ttu-id="f1637-232">Le origini di configurazione vengono lette nell'ordine in cui vengono specificati i rispetti provider di configurazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="f1637-232">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="f1637-233">I provider di configurazione descritti in questo argomento sono descritti in ordine alfabetico, non nell'ordine in cui il codice li dispone.</span><span class="sxs-lookup"><span data-stu-id="f1637-233">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="f1637-234">Ordinare i provider di configurazione nel codice in base alle priorità per le origini di configurazione sottostanti richieste dall'app.</span><span class="sxs-lookup"><span data-stu-id="f1637-234">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="f1637-235">Una sequenza tipica di provider di configurazione è:</span><span class="sxs-lookup"><span data-stu-id="f1637-235">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="f1637-236">File (*appsettings.json*, *appsettings.{Environment}.json*, dove `{Environment}` è l'ambiente di hosting corrente dell'app)</span><span class="sxs-lookup"><span data-stu-id="f1637-236">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="f1637-237">Insieme di credenziali delle chiavi di Azure</span><span class="sxs-lookup"><span data-stu-id="f1637-237">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="f1637-238">[Segreti utente (Secret Manager)](xref:security/app-secrets) (solo nell'ambiente di sviluppo)</span><span class="sxs-lookup"><span data-stu-id="f1637-238">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="f1637-239">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="f1637-239">Environment variables</span></span>
1. <span data-ttu-id="f1637-240">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="f1637-240">Command-line arguments</span></span>

<span data-ttu-id="f1637-241">È pratica comune posizionare il provider di configurazione della riga di comando per ultimo in una serie di provider per consentire agli argomenti della riga di comando di sostituire la configurazione impostata da altri provider.</span><span class="sxs-lookup"><span data-stu-id="f1637-241">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="f1637-242">La sequenza di provider precedente viene utilizzata quando un nuovo generatore host viene inizializzato con `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f1637-242">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="f1637-243">Per altre informazioni, vedere la sezione [Configurazione predefinita](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="f1637-243">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="f1637-244">Configurare il generatore di host con ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="f1637-244">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="f1637-245">Per configurare il generatore di host, chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> e specificare la configurazione.</span><span class="sxs-lookup"><span data-stu-id="f1637-245">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="f1637-246">`ConfigureHostConfiguration` viene usato per inizializzare l'oggetto <xref:Microsoft.Extensions.Hosting.IHostEnvironment> da usare in un secondo momento nel processo di compilazione.</span><span class="sxs-lookup"><span data-stu-id="f1637-246">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="f1637-247">`ConfigureHostConfiguration` può essere chiamato più volte e i risultati si sommano.</span><span class="sxs-lookup"><span data-stu-id="f1637-247">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

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

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="f1637-248">Configurare il generatore di host con UseConfiguration</span><span class="sxs-lookup"><span data-stu-id="f1637-248">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="f1637-249">Per configurare il generatore di host, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> sul generatore di host con la configurazione.</span><span class="sxs-lookup"><span data-stu-id="f1637-249">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

## <a name="configureappconfiguration"></a><span data-ttu-id="f1637-250">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="f1637-250">ConfigureAppConfiguration</span></span>

<span data-ttu-id="f1637-251">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare i provider di configurazione dell'app, oltre a quelli aggiunti automaticamente da `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="f1637-251">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="f1637-252">Sostituire la configurazione precedente con argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="f1637-252">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="f1637-253">Per fornire la configurazione dell'app che può essere sostituita con argomenti della riga di comando, chiamare `AddCommandLine` per ultimo:</span><span class="sxs-lookup"><span data-stu-id="f1637-253">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="f1637-254">Rimuovere i provider aggiunti da CreateDefaultBuilder</span><span class="sxs-lookup"><span data-stu-id="f1637-254">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="f1637-255">Per rimuovere i provider aggiunti da `CreateDefaultBuilder`, chiamare prima [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) in [IConfigurationBuilder. Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) :</span><span class="sxs-lookup"><span data-stu-id="f1637-255">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="f1637-256">Utilizzare la configurazione durante l'avvio dell'app</span><span class="sxs-lookup"><span data-stu-id="f1637-256">Consume configuration during app startup</span></span>

<span data-ttu-id="f1637-257">La configurazione fornita all'app in `ConfigureAppConfiguration` è disponibile durante l'avvio dell'app, incluso `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f1637-257">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="f1637-258">Per altre informazioni, vedere la sezione [Accedere alla configurazione durante l'avvio](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="f1637-258">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="f1637-259">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="f1637-259">Command-line Configuration Provider</span></span>

<span data-ttu-id="f1637-260"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> carica la configurazione da coppie chiave-valore di argomenti della riga di comando in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="f1637-260">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="f1637-261">Per attivare la configurazione della riga di comando, metodo di estensione <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> viene chiamato su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f1637-261">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f1637-262">`AddCommandLine` viene chiamato automaticamente quando viene chiamato il metodo `CreateDefaultBuilder(string [])`.</span><span class="sxs-lookup"><span data-stu-id="f1637-262">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="f1637-263">Per altre informazioni, vedere la sezione [Configurazione predefinita](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="f1637-263">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="f1637-264">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="f1637-264">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="f1637-265">Una configurazione facoltativa dai file *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="f1637-265">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="f1637-266">[Segreti utente (Secret Manager)](xref:security/app-secrets) nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="f1637-266">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="f1637-267">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="f1637-267">Environment variables.</span></span>

<span data-ttu-id="f1637-268">`CreateDefaultBuilder` aggiunge il provider di configurazione della riga di comando per ultimo.</span><span class="sxs-lookup"><span data-stu-id="f1637-268">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="f1637-269">Gli argomenti della riga di comando passati in fase di esecuzione sostituiscono la configurazione impostata dagli altri provider.</span><span class="sxs-lookup"><span data-stu-id="f1637-269">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="f1637-270">`CreateDefaultBuilder` opera durante la creazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="f1637-270">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="f1637-271">Pertanto, la configurazione della riga di comando attivata da `CreateDefaultBuilder` può influire sul modo in cui l'host viene configurato.</span><span class="sxs-lookup"><span data-stu-id="f1637-271">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="f1637-272">Per le app basate sui modelli di ASP.NET Core, `AddCommandLine` è già stato chiamato da `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f1637-272">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="f1637-273">Per aggiungere altri provider di configurazione e mantenere la possibilità di sostituire la configurazione di tali provider con argomenti della riga di comando, chiamare i provider aggiuntivi dell'app in `ConfigureAppConfiguration` e quindi chiamare `AddCommandLine` per ultimo.</span><span class="sxs-lookup"><span data-stu-id="f1637-273">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="f1637-274">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="f1637-274">**Example**</span></span>

<span data-ttu-id="f1637-275">L'app di esempio consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include una chiamata a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="f1637-275">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="f1637-276">Aprire un prompt dei comandi nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="f1637-276">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="f1637-277">Fornire un argomento della riga di comando per il comando `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="f1637-277">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="f1637-278">Dopo che l'app è in esecuzione, aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f1637-278">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="f1637-279">Notare che l'output contiene la coppia chiave-valore per l'argomento della riga di comando di configurazione fornito per `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="f1637-279">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="f1637-280">Argomenti</span><span class="sxs-lookup"><span data-stu-id="f1637-280">Arguments</span></span>

<span data-ttu-id="f1637-281">Il valore deve seguire un segno di uguale (`=`) o la chiave deve avere un prefisso (`--` o `/`) quando il valore segue uno spazio.</span><span class="sxs-lookup"><span data-stu-id="f1637-281">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="f1637-282">Il valore non è necessario se viene usato un segno di uguale, ad esempio `CommandLineKey=`.</span><span class="sxs-lookup"><span data-stu-id="f1637-282">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="f1637-283">Prefisso chiave</span><span class="sxs-lookup"><span data-stu-id="f1637-283">Key prefix</span></span>               | <span data-ttu-id="f1637-284">Esempio</span><span class="sxs-lookup"><span data-stu-id="f1637-284">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="f1637-285">Nessun prefisso</span><span class="sxs-lookup"><span data-stu-id="f1637-285">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="f1637-286">Due trattini (`--`)</span><span class="sxs-lookup"><span data-stu-id="f1637-286">Two dashes (`--`)</span></span>        | <span data-ttu-id="f1637-287">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="f1637-287">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="f1637-288">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="f1637-288">Forward slash (`/`)</span></span>      | <span data-ttu-id="f1637-289">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="f1637-289">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="f1637-290">Nello stesso comando, non mischiare coppie chiave-valore di argomenti della riga di comando che usano un segno di uguale con coppie chiave-valore che usano uno spazio.</span><span class="sxs-lookup"><span data-stu-id="f1637-290">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="f1637-291">Comandi di esempio:</span><span class="sxs-lookup"><span data-stu-id="f1637-291">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="f1637-292">Mapping di sostituzione</span><span class="sxs-lookup"><span data-stu-id="f1637-292">Switch mappings</span></span>

<span data-ttu-id="f1637-293">I mapping di sostituzione consentono di usare la logica di sostituzione del nome della chiave.</span><span class="sxs-lookup"><span data-stu-id="f1637-293">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="f1637-294">Quando si compila manualmente la configurazione con una <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, fornire un dizionario di sostituzioni switch al metodo <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="f1637-294">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="f1637-295">Quando viene utilizzato il dizionario dei mapping di sostituzione, nel dizionario viene controllata la presenza di una chiave corrispondente alla chiave fornita da un argomento della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="f1637-295">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="f1637-296">Se la chiave della riga di comando viene trovata nel dizionario, il valore del dizionario (la sostituzione della chiave) viene passato nuovamente per impostare la coppia chiave-valore nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="f1637-296">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="f1637-297">Un mapping di sostituzione è necessario per le chiavi della riga di comando con un trattino singolo (`-`) come prefisso.</span><span class="sxs-lookup"><span data-stu-id="f1637-297">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="f1637-298">Regole principali del dizionario dei mapping di sostituzione:</span><span class="sxs-lookup"><span data-stu-id="f1637-298">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="f1637-299">Le sostituzioni devono iniziare con un trattino (`-`) o un trattino doppio (`--`).</span><span class="sxs-lookup"><span data-stu-id="f1637-299">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="f1637-300">Il dizionario dei mapping di sostituzione non deve contenere chiavi duplicate.</span><span class="sxs-lookup"><span data-stu-id="f1637-300">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="f1637-301">Creare un dizionario dei mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="f1637-301">Create a switch mappings dictionary.</span></span> <span data-ttu-id="f1637-302">Nell'esempio seguente vengono creati due mapping di sostituzione:</span><span class="sxs-lookup"><span data-stu-id="f1637-302">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="f1637-303">Quando viene creato l'host, chiamare `AddCommandLine` con il dizionario dei mapping di sostituzione:</span><span class="sxs-lookup"><span data-stu-id="f1637-303">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="f1637-304">Per le app che usano i mapping di sostituzione, la chiamata a `CreateDefaultBuilder` non deve passare argomenti.</span><span class="sxs-lookup"><span data-stu-id="f1637-304">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="f1637-305">La chiamata `AddCommandLine` del metodo `CreateDefaultBuilder` non include sostituzioni mappate e non è possibile passare il dizionario dei mapping di sostituzione a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f1637-305">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="f1637-306">La soluzione consiste nel non passare gli argomenti a `CreateDefaultBuilder` consentendo invece al metodo `AddCommandLine` del metodo `ConfigurationBuilder` di elaborare entrambi gli argomenti e il dizionario dei mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="f1637-306">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="f1637-307">Il dizionario dei mapping di sostituzione creato contiene i dati visualizzati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="f1637-307">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="f1637-308">Key</span><span class="sxs-lookup"><span data-stu-id="f1637-308">Key</span></span>       | <span data-ttu-id="f1637-309">Valore</span><span class="sxs-lookup"><span data-stu-id="f1637-309">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="f1637-310">Se le chiavi con mapping di sostituzione vengono usate all'avvio dell'app, la configurazione riceve il valore di configurazione per la chiave fornita dal dizionario:</span><span class="sxs-lookup"><span data-stu-id="f1637-310">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="f1637-311">Dopo aver eseguito il comando precedente, la configurazione contiene i valori mostrati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="f1637-311">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="f1637-312">Key</span><span class="sxs-lookup"><span data-stu-id="f1637-312">Key</span></span>               | <span data-ttu-id="f1637-313">Valore</span><span class="sxs-lookup"><span data-stu-id="f1637-313">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="f1637-314">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="f1637-314">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="f1637-315"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> carica la configurazione da coppie chiave-valore di variabili di ambiente in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="f1637-315">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="f1637-316">Per attivare la configurazione delle variabili di ambiente, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f1637-316">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="f1637-317">[App Azure servizio](https://azure.microsoft.com/services/app-service/) consente di impostare le variabili di ambiente nel portale di Azure che possono eseguire l'override della configurazione dell'app usando il provider di configurazione delle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="f1637-317">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="f1637-318">Per altre informazioni, vedere [App di Azure: Eseguire l'override della configurazione delle app usando il portale di Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="f1637-318">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f1637-319">`AddEnvironmentVariables` consente di caricare variabili di ambiente con prefisso `DOTNET_` per la [configurazione host](#host-versus-app-configuration) quando viene inizializzato un nuovo generatore di host con l'[host generico](xref:fundamentals/host/generic-host) e viene chiamato `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f1637-319">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="f1637-320">Per altre informazioni, vedere la sezione [Configurazione predefinita](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="f1637-320">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f1637-321">`AddEnvironmentVariables` consente di caricare variabili di ambiente con prefisso `ASPNETCORE_` per la [configurazione host](#host-versus-app-configuration) quando viene inizializzato un nuovo generatore di host con l'[host Web](xref:fundamentals/host/web-host) e viene chiamato `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f1637-321">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="f1637-322">Per altre informazioni, vedere la sezione [Configurazione predefinita](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="f1637-322">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

<span data-ttu-id="f1637-323">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="f1637-323">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="f1637-324">Configurazione delle app dalle variabili di ambiente senza prefisso chiamando `AddEnvironmentVariables` senza prefisso.</span><span class="sxs-lookup"><span data-stu-id="f1637-324">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="f1637-325">Una configurazione facoltativa dai file *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="f1637-325">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="f1637-326">[Segreti utente (Secret Manager)](xref:security/app-secrets) nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="f1637-326">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="f1637-327">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="f1637-327">Command-line arguments.</span></span>

<span data-ttu-id="f1637-328">Il provider di configurazione delle variabili di ambiente viene chiamato dopo aver stabilito la configurazione dai segreti utente e dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="f1637-328">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="f1637-329">La chiamata del provider in questa posizione consente alle variabili di ambiente lette in fase di esecuzione di sostituire la configurazione impostata dai segreti utente e dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="f1637-329">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="f1637-330">Per fornire la configurazione dell'app da variabili di ambiente aggiuntive, chiamare i provider aggiuntivi dell'app in `ConfigureAppConfiguration` e chiamare `AddEnvironmentVariables` con il prefisso:</span><span class="sxs-lookup"><span data-stu-id="f1637-330">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="f1637-331">Chiamare `AddEnvironmentVariables` ultimo per consentire alle variabili di ambiente con il prefisso specificato di eseguire l'override dei valori di altri provider.</span><span class="sxs-lookup"><span data-stu-id="f1637-331">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="f1637-332">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="f1637-332">**Example**</span></span>

<span data-ttu-id="f1637-333">L'app di esempio consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include una chiamata a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="f1637-333">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="f1637-334">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="f1637-334">Run the sample app.</span></span> <span data-ttu-id="f1637-335">Aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f1637-335">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="f1637-336">Notare che l'output contiene la coppia chiave-valore per la variabile di ambiente `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="f1637-336">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="f1637-337">Il valore riflette l'ambiente in cui l'app è in esecuzione, in genere `Development` durante l'esecuzione locale.</span><span class="sxs-lookup"><span data-stu-id="f1637-337">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="f1637-338">Per limitare l'elenco delle variabili di ambiente restituito dall'app, l'app filtra le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="f1637-338">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="f1637-339">Vedere il file *Pages/Index.cshtml.cs* dell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="f1637-339">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="f1637-340">Per esporre tutte le variabili di ambiente disponibili per l'app, modificare il `FilteredConfiguration` in *pages/index. cshtml. cs* come segue:</span><span class="sxs-lookup"><span data-stu-id="f1637-340">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="f1637-341">Prefissi</span><span class="sxs-lookup"><span data-stu-id="f1637-341">Prefixes</span></span>

<span data-ttu-id="f1637-342">Le variabili di ambiente caricate nella configurazione dell'app vengono filtrate quando si specifica un prefisso per il metodo `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="f1637-342">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="f1637-343">Ad esempio, per filtrare le variabili di ambiente in base al prefisso `CUSTOM_`, fornire il prefisso al provider di configurazione:</span><span class="sxs-lookup"><span data-stu-id="f1637-343">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="f1637-344">Il prefisso viene rimosso quando vengono create le coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="f1637-344">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="f1637-345">Quando viene creato il generatore di host, la configurazione dell'host viene fornita dalle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="f1637-345">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="f1637-346">Per altre informazioni sul prefisso usato per queste variabili di ambiente, vedere la sezione [Configurazione predefinita](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="f1637-346">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="f1637-347">**Prefissi della stringa di connessione**</span><span class="sxs-lookup"><span data-stu-id="f1637-347">**Connection string prefixes**</span></span>

<span data-ttu-id="f1637-348">L'API di configurazione prevede regole di elaborazione speciali per quattro variabili di ambiente di stringa di connessione coinvolte nella configurazione di stringhe di connessione di Azure per l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="f1637-348">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="f1637-349">Le variabili di ambiente con i prefissi indicati nella tabella vengono caricate nell'app se non viene fornito alcun prefisso a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="f1637-349">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="f1637-350">Prefisso della stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="f1637-350">Connection string prefix</span></span> | <span data-ttu-id="f1637-351">Provider</span><span class="sxs-lookup"><span data-stu-id="f1637-351">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="f1637-352">Provider personalizzato</span><span class="sxs-lookup"><span data-stu-id="f1637-352">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="f1637-353">MySQL</span><span class="sxs-lookup"><span data-stu-id="f1637-353">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="f1637-354">Database SQL di Azure</span><span class="sxs-lookup"><span data-stu-id="f1637-354">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="f1637-355">SQL Server</span><span class="sxs-lookup"><span data-stu-id="f1637-355">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="f1637-356">Quando una variabile di ambiente viene individuata e caricata nella configurazione con uno qualsiasi dei quattro prefissi indicati nella tabella:</span><span class="sxs-lookup"><span data-stu-id="f1637-356">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="f1637-357">La chiave di configurazione viene creata rimuovendo il prefisso della variabile di ambiente e aggiungendo una sezione per la chiave di configurazione (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="f1637-357">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="f1637-358">Viene creata una nuova coppia chiave-valore della configurazione che rappresenta il provider di connessione al database (ad eccezione di `CUSTOMCONNSTR_`, che non ha un provider dichiarato).</span><span class="sxs-lookup"><span data-stu-id="f1637-358">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="f1637-359">Chiave della variabile di ambiente</span><span class="sxs-lookup"><span data-stu-id="f1637-359">Environment variable key</span></span> | <span data-ttu-id="f1637-360">Chiave di configurazione convertita</span><span class="sxs-lookup"><span data-stu-id="f1637-360">Converted configuration key</span></span> | <span data-ttu-id="f1637-361">Voce di configurazione del provider</span><span class="sxs-lookup"><span data-stu-id="f1637-361">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="f1637-362">Voce di configurazione non creata.</span><span class="sxs-lookup"><span data-stu-id="f1637-362">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="f1637-363">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="f1637-363">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="f1637-364">Valore: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="f1637-364">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="f1637-365">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="f1637-365">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="f1637-366">Valore: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="f1637-366">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="f1637-367">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="f1637-367">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="f1637-368">Valore: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="f1637-368">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="f1637-369">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="f1637-369">File Configuration Provider</span></span>

<span data-ttu-id="f1637-370"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> è la classe base per il caricamento della configurazione dal file system.</span><span class="sxs-lookup"><span data-stu-id="f1637-370"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="f1637-371">I provider di configurazione seguenti sono dedicati per specifici tipi di file:</span><span class="sxs-lookup"><span data-stu-id="f1637-371">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="f1637-372">Provider di configurazione INI</span><span class="sxs-lookup"><span data-stu-id="f1637-372">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="f1637-373">Provider di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="f1637-373">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="f1637-374">Provider di configurazione XML</span><span class="sxs-lookup"><span data-stu-id="f1637-374">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="f1637-375">Provider di configurazione INI</span><span class="sxs-lookup"><span data-stu-id="f1637-375">INI Configuration Provider</span></span>

<span data-ttu-id="f1637-376"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> carica la configurazione da coppie chiave-valore di file INI in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="f1637-376">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="f1637-377">Per attivare la configurazione di file INI, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f1637-377">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f1637-378">I due punti possono essere usati come delimitatore di sezione nella configurazione di file INI.</span><span class="sxs-lookup"><span data-stu-id="f1637-378">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="f1637-379">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="f1637-379">Overloads permit specifying:</span></span>

* <span data-ttu-id="f1637-380">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="f1637-380">Whether the file is optional.</span></span>
* <span data-ttu-id="f1637-381">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="f1637-381">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="f1637-382">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="f1637-382">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="f1637-383">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="f1637-383">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="f1637-384">Un esempio generico di un file di configurazione INI:</span><span class="sxs-lookup"><span data-stu-id="f1637-384">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="f1637-385">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="f1637-385">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="f1637-386">section0:key0</span><span class="sxs-lookup"><span data-stu-id="f1637-386">section0:key0</span></span>
* <span data-ttu-id="f1637-387">section0:key1</span><span class="sxs-lookup"><span data-stu-id="f1637-387">section0:key1</span></span>
* <span data-ttu-id="f1637-388">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="f1637-388">section1:subsection:key</span></span>
* <span data-ttu-id="f1637-389">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="f1637-389">section2:subsection0:key</span></span>
* <span data-ttu-id="f1637-390">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="f1637-390">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="f1637-391">Provider di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="f1637-391">JSON Configuration Provider</span></span>

<span data-ttu-id="f1637-392">Il <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> carica la configurazione da coppie chiave-valore di file JSON in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="f1637-392">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="f1637-393">Per attivare la configurazione di file JSON, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f1637-393">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f1637-394">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="f1637-394">Overloads permit specifying:</span></span>

* <span data-ttu-id="f1637-395">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="f1637-395">Whether the file is optional.</span></span>
* <span data-ttu-id="f1637-396">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="f1637-396">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="f1637-397">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="f1637-397">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="f1637-398">`AddJsonFile` viene chiamato automaticamente due volte quando un nuovo generatore host viene inizializzato con `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f1637-398">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="f1637-399">Il metodo viene chiamato per caricare la configurazione da:</span><span class="sxs-lookup"><span data-stu-id="f1637-399">The method is called to load configuration from:</span></span>

* <span data-ttu-id="f1637-400">*appSettings. json* &ndash; questo file viene letto per primo.</span><span class="sxs-lookup"><span data-stu-id="f1637-400">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="f1637-401">La versione dell'ambiente del file può sostituire i valori forniti dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f1637-401">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="f1637-402">*appSettings. {Environment}. JSON* &ndash; la versione dell'ambiente del file viene caricata in base a [IHostingEnvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="f1637-402">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="f1637-403">Per altre informazioni, vedere la sezione [Configurazione predefinita](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="f1637-403">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="f1637-404">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="f1637-404">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="f1637-405">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="f1637-405">Environment variables.</span></span>
* <span data-ttu-id="f1637-406">[Segreti utente (Secret Manager)](xref:security/app-secrets) nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="f1637-406">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="f1637-407">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="f1637-407">Command-line arguments.</span></span>

<span data-ttu-id="f1637-408">Il provider di configurazione JSON viene stabilito per primo.</span><span class="sxs-lookup"><span data-stu-id="f1637-408">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="f1637-409">I segreti utente, le variabili di ambiente e gli argomenti della riga di comando sostituiscono quindi la configurazione impostata dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="f1637-409">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="f1637-410">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare la configurazione dell'app per file diversi da *appsettings.json* e *appsettings.{Environment}.json*:</span><span class="sxs-lookup"><span data-stu-id="f1637-410">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="f1637-411">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="f1637-411">**Example**</span></span>

<span data-ttu-id="f1637-412">L'app di esempio sfrutta il metodo di convenienza statica `CreateDefaultBuilder` per compilare l'host, che include due chiamate al `AddJsonFile`:</span><span class="sxs-lookup"><span data-stu-id="f1637-412">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="f1637-413">La prima chiamata a `AddJsonFile` carica la configurazione da *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="f1637-413">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="f1637-414">La seconda chiamata a `AddJsonFile` carica la configurazione da *appSettings. { Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="f1637-414">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="f1637-415">Per *appSettings. Development. JSON* nell'app di esempio, viene caricato il file seguente:</span><span class="sxs-lookup"><span data-stu-id="f1637-415">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="f1637-416">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="f1637-416">Run the sample app.</span></span> <span data-ttu-id="f1637-417">Aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f1637-417">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="f1637-418">L'output contiene coppie chiave-valore per la configurazione basata sull'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="f1637-418">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="f1637-419">Il livello di registrazione per la chiave `Logging:LogLevel:Default` viene `Debug` quando si esegue l'app nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="f1637-419">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="f1637-420">Eseguire di nuovo l'app di esempio nell'ambiente di produzione:</span><span class="sxs-lookup"><span data-stu-id="f1637-420">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="f1637-421">Aprire il file *Properties/launchSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="f1637-421">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="f1637-422">Nel profilo di `ConfigurationSample` modificare il valore della variabile di ambiente `ASPNETCORE_ENVIRONMENT` in `Production`.</span><span class="sxs-lookup"><span data-stu-id="f1637-422">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="f1637-423">Salvare il file ed eseguire l'app con `dotnet run` in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="f1637-423">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="f1637-424">Impostazioni in *appSettings. Development. JSON* non sostituisce più le impostazioni in *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="f1637-424">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="f1637-425">Il livello di registrazione per la chiave `Logging:LogLevel:Default` è `Information`.</span><span class="sxs-lookup"><span data-stu-id="f1637-425">The log level for the key `Logging:LogLevel:Default` is `Information`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="f1637-426">La prima chiamata a `AddJsonFile` carica la configurazione da *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="f1637-426">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="f1637-427">La seconda chiamata a `AddJsonFile` carica la configurazione da *appSettings. { Environment}. JSON*.</span><span class="sxs-lookup"><span data-stu-id="f1637-427">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="f1637-428">Per *appSettings. Development. JSON* nell'app di esempio, viene caricato il file seguente:</span><span class="sxs-lookup"><span data-stu-id="f1637-428">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="f1637-429">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="f1637-429">Run the sample app.</span></span> <span data-ttu-id="f1637-430">Aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f1637-430">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="f1637-431">L'output contiene coppie chiave-valore per la configurazione basata sull'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="f1637-431">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="f1637-432">Il livello di registrazione per la chiave `Logging:LogLevel:Default` viene `Debug` quando si esegue l'app nell'ambiente di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="f1637-432">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="f1637-433">Eseguire di nuovo l'app di esempio nell'ambiente di produzione:</span><span class="sxs-lookup"><span data-stu-id="f1637-433">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="f1637-434">Aprire il file *Properties/launchSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="f1637-434">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="f1637-435">Nel profilo di `ConfigurationSample` modificare il valore della variabile di ambiente `ASPNETCORE_ENVIRONMENT` in `Production`.</span><span class="sxs-lookup"><span data-stu-id="f1637-435">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="f1637-436">Salvare il file ed eseguire l'app con `dotnet run` in una shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="f1637-436">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="f1637-437">Impostazioni in *appSettings. Development. JSON* non sostituisce più le impostazioni in *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="f1637-437">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="f1637-438">Il livello di registrazione per la chiave `Logging:LogLevel:Default` è `Warning`.</span><span class="sxs-lookup"><span data-stu-id="f1637-438">The log level for the key `Logging:LogLevel:Default` is `Warning`.</span></span>

::: moniker-end

### <a name="xml-configuration-provider"></a><span data-ttu-id="f1637-439">Provider di configurazione XML</span><span class="sxs-lookup"><span data-stu-id="f1637-439">XML Configuration Provider</span></span>

<span data-ttu-id="f1637-440">Il <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> carica la configurazione da coppie chiave-valore di file XML in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="f1637-440">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="f1637-441">Per attivare la configurazione di file XML, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f1637-441">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f1637-442">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="f1637-442">Overloads permit specifying:</span></span>

* <span data-ttu-id="f1637-443">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="f1637-443">Whether the file is optional.</span></span>
* <span data-ttu-id="f1637-444">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="f1637-444">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="f1637-445">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="f1637-445">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="f1637-446">Il nodo radice del file di configurazione viene ignorato quando vengono create le coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="f1637-446">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="f1637-447">Non specificare una definizione DTD (Document Type Definition) o uno spazio dei nomi nel file.</span><span class="sxs-lookup"><span data-stu-id="f1637-447">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="f1637-448">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="f1637-448">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="f1637-449">I file di configurazione XML possono usare nomi di elementi distinti per le sezioni ripetute:</span><span class="sxs-lookup"><span data-stu-id="f1637-449">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="f1637-450">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="f1637-450">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="f1637-451">section0:key0</span><span class="sxs-lookup"><span data-stu-id="f1637-451">section0:key0</span></span>
* <span data-ttu-id="f1637-452">section0:key1</span><span class="sxs-lookup"><span data-stu-id="f1637-452">section0:key1</span></span>
* <span data-ttu-id="f1637-453">section1:key0</span><span class="sxs-lookup"><span data-stu-id="f1637-453">section1:key0</span></span>
* <span data-ttu-id="f1637-454">section1:key1</span><span class="sxs-lookup"><span data-stu-id="f1637-454">section1:key1</span></span>

<span data-ttu-id="f1637-455">La ripetizione di elementi che usano lo stesso nome di elemento funziona se si usa l'attributo `name` per distinguere gli elementi:</span><span class="sxs-lookup"><span data-stu-id="f1637-455">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="f1637-456">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="f1637-456">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="f1637-457">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="f1637-457">section:section0:key:key0</span></span>
* <span data-ttu-id="f1637-458">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="f1637-458">section:section0:key:key1</span></span>
* <span data-ttu-id="f1637-459">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="f1637-459">section:section1:key:key0</span></span>
* <span data-ttu-id="f1637-460">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="f1637-460">section:section1:key:key1</span></span>

<span data-ttu-id="f1637-461">È possibile usare attributi per fornire valori:</span><span class="sxs-lookup"><span data-stu-id="f1637-461">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="f1637-462">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="f1637-462">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="f1637-463">key:attribute</span><span class="sxs-lookup"><span data-stu-id="f1637-463">key:attribute</span></span>
* <span data-ttu-id="f1637-464">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="f1637-464">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="f1637-465">Provider di configurazione KeyPerFile</span><span class="sxs-lookup"><span data-stu-id="f1637-465">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="f1637-466">Il <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> usa i file di una directory come coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="f1637-466">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="f1637-467">La chiave è il nome del file.</span><span class="sxs-lookup"><span data-stu-id="f1637-467">The key is the file name.</span></span> <span data-ttu-id="f1637-468">Il valore contiene il contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="f1637-468">The value contains the file's contents.</span></span> <span data-ttu-id="f1637-469">Il provider di configurazione KeyPerFile viene usato in scenari di hosting Docker.</span><span class="sxs-lookup"><span data-stu-id="f1637-469">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="f1637-470">Per attivare la configurazione chiave-per-file, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f1637-470">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="f1637-471">Il `directoryPath` per i file deve essere un percorso assoluto.</span><span class="sxs-lookup"><span data-stu-id="f1637-471">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="f1637-472">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="f1637-472">Overloads permit specifying:</span></span>

* <span data-ttu-id="f1637-473">Un delegato `Action<KeyPerFileConfigurationSource>` che configura l'origine.</span><span class="sxs-lookup"><span data-stu-id="f1637-473">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="f1637-474">Se la directory è facoltativa e il percorso della directory.</span><span class="sxs-lookup"><span data-stu-id="f1637-474">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="f1637-475">Il doppio carattere di sottolineatura (`__`) viene usato come delimitatore per le chiavi di configurazione nei nomi dei file.</span><span class="sxs-lookup"><span data-stu-id="f1637-475">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="f1637-476">Ad esempio, il nome di file `Logging__LogLevel__System` produce la chiave di configurazione `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="f1637-476">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="f1637-477">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="f1637-477">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="f1637-478">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="f1637-478">Memory Configuration Provider</span></span>

<span data-ttu-id="f1637-479">Il <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> usa una raccolta in memoria come coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="f1637-479">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="f1637-480">Per attivare la configurazione della raccolta in memoria, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f1637-480">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f1637-481">Il provider di configurazione può essere inizializzato con un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="f1637-481">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="f1637-482">Chiamare `ConfigureAppConfiguration` quando si crea l'host per specificare la configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="f1637-482">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="f1637-483">Nell'esempio seguente viene creato un dizionario di configurazione:</span><span class="sxs-lookup"><span data-stu-id="f1637-483">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="f1637-484">Il dizionario viene usato con una chiamata a `AddInMemoryCollection` per fornire la configurazione:</span><span class="sxs-lookup"><span data-stu-id="f1637-484">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="f1637-485">GetValue</span><span class="sxs-lookup"><span data-stu-id="f1637-485">GetValue</span></span>

<span data-ttu-id="f1637-486">[ConfigurationBinder. GetValue\<t >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) estrae un singolo valore dalla configurazione con una chiave specificata e lo converte nel tipo non di raccolta specificato.</span><span class="sxs-lookup"><span data-stu-id="f1637-486">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="f1637-487">Un overload accetta un valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="f1637-487">An overload accepts a default value.</span></span>

<span data-ttu-id="f1637-488">L'esempio seguente consente di:</span><span class="sxs-lookup"><span data-stu-id="f1637-488">The following example:</span></span>

* <span data-ttu-id="f1637-489">Estrae il valore di stringa dalla configurazione con la chiave `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="f1637-489">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="f1637-490">Se `NumberKey` non viene trovato nelle chiavi di configurazione, viene usato il valore predefinito di `99`.</span><span class="sxs-lookup"><span data-stu-id="f1637-490">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="f1637-491">Assegna al valore il tipo `int`.</span><span class="sxs-lookup"><span data-stu-id="f1637-491">Types the value as an `int`.</span></span>
* <span data-ttu-id="f1637-492">Memorizza il valore nella proprietà `NumberConfig` per l'uso da parte della pagina.</span><span class="sxs-lookup"><span data-stu-id="f1637-492">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="f1637-493">GetSection, GetChildren ed Exists</span><span class="sxs-lookup"><span data-stu-id="f1637-493">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="f1637-494">Per gli esempi che seguono, prendere in considerazione il file JSON seguente.</span><span class="sxs-lookup"><span data-stu-id="f1637-494">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="f1637-495">Vengono trovate quattro chiavi in due sezioni, una delle quali include una coppia di sottosezioni:</span><span class="sxs-lookup"><span data-stu-id="f1637-495">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="f1637-496">Quando il file viene letto nella configurazione, vengono create le chiavi gerarchiche univoche seguenti per contenere i valori di configurazione:</span><span class="sxs-lookup"><span data-stu-id="f1637-496">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="f1637-497">section0:key0</span><span class="sxs-lookup"><span data-stu-id="f1637-497">section0:key0</span></span>
* <span data-ttu-id="f1637-498">section0:key1</span><span class="sxs-lookup"><span data-stu-id="f1637-498">section0:key1</span></span>
* <span data-ttu-id="f1637-499">section1:key0</span><span class="sxs-lookup"><span data-stu-id="f1637-499">section1:key0</span></span>
* <span data-ttu-id="f1637-500">section1:key1</span><span class="sxs-lookup"><span data-stu-id="f1637-500">section1:key1</span></span>
* <span data-ttu-id="f1637-501">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="f1637-501">section2:subsection0:key0</span></span>
* <span data-ttu-id="f1637-502">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="f1637-502">section2:subsection0:key1</span></span>
* <span data-ttu-id="f1637-503">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="f1637-503">section2:subsection1:key0</span></span>
* <span data-ttu-id="f1637-504">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="f1637-504">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="f1637-505">GetSection</span><span class="sxs-lookup"><span data-stu-id="f1637-505">GetSection</span></span>

<span data-ttu-id="f1637-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) estrae una sottosezione della configurazione con la chiave di sottosezione specificata.</span><span class="sxs-lookup"><span data-stu-id="f1637-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="f1637-507">Per restituire una <xref:Microsoft.Extensions.Configuration.IConfigurationSection> che contiene solo le coppie chiave-valore in `section1`, chiamare `GetSection` e fornire il nome della sezione:</span><span class="sxs-lookup"><span data-stu-id="f1637-507">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="f1637-508">`configSection` non ha un valore, ma solo una chiave e un percorso.</span><span class="sxs-lookup"><span data-stu-id="f1637-508">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="f1637-509">In modo analogo, per ottenere i valori per le chiavi in `section2:subsection0`, chiamare `GetSection` e fornire il percorso della sezione:</span><span class="sxs-lookup"><span data-stu-id="f1637-509">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="f1637-510">`GetSection` non restituisce mai `null`.</span><span class="sxs-lookup"><span data-stu-id="f1637-510">`GetSection` never returns `null`.</span></span> <span data-ttu-id="f1637-511">Se non viene trovata una sezione corrispondente, viene restituita una `IConfigurationSection` vuota.</span><span class="sxs-lookup"><span data-stu-id="f1637-511">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="f1637-512">Quando `GetSection` restituisce una sezione corrispondente, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> non viene compilata.</span><span class="sxs-lookup"><span data-stu-id="f1637-512">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="f1637-513">Quando la sezione esiste, vengono restituiti <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> e <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path>.</span><span class="sxs-lookup"><span data-stu-id="f1637-513">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="f1637-514">GetChildren</span><span class="sxs-lookup"><span data-stu-id="f1637-514">GetChildren</span></span>

<span data-ttu-id="f1637-515">Una chiamata a [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) per `section2` ottiene una `IEnumerable<IConfigurationSection>` che include:</span><span class="sxs-lookup"><span data-stu-id="f1637-515">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="f1637-516">Esistente</span><span class="sxs-lookup"><span data-stu-id="f1637-516">Exists</span></span>

<span data-ttu-id="f1637-517">Usare [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) per determinare se esiste una sezione di configurazione:</span><span class="sxs-lookup"><span data-stu-id="f1637-517">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="f1637-518">Considerati i dati di esempio, `sectionExists` è `false` perché non esiste una sezione `section2:subsection2` nei dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f1637-518">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="f1637-519">Eseguire l'associazione a una classe</span><span class="sxs-lookup"><span data-stu-id="f1637-519">Bind to a class</span></span>

<span data-ttu-id="f1637-520">La configurazione può essere associata alle classi che rappresentano gruppi di impostazioni correlate usando il *modello di opzioni*.</span><span class="sxs-lookup"><span data-stu-id="f1637-520">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="f1637-521">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="f1637-521">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="f1637-522">I valori di configurazione vengono restituiti come stringhe, ma la chiamata di <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> consente la costruzione di oggetti [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="f1637-522">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="f1637-523">L'app di esempio contiene un modello `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="f1637-523">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f1637-524">La sezione `starship` del file *starship.json* crea la configurazione quando l'app di esempio usa il provider di configurazione JSON per caricare la configurazione:</span><span class="sxs-lookup"><span data-stu-id="f1637-524">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="f1637-525">Vengono create le coppie chiave-valore della configurazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="f1637-525">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="f1637-526">Key</span><span class="sxs-lookup"><span data-stu-id="f1637-526">Key</span></span>                   | <span data-ttu-id="f1637-527">Valore</span><span class="sxs-lookup"><span data-stu-id="f1637-527">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="f1637-528">starship:name</span><span class="sxs-lookup"><span data-stu-id="f1637-528">starship:name</span></span>         | <span data-ttu-id="f1637-529">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="f1637-529">USS Enterprise</span></span>                                    |
| <span data-ttu-id="f1637-530">starship:registry</span><span class="sxs-lookup"><span data-stu-id="f1637-530">starship:registry</span></span>     | <span data-ttu-id="f1637-531">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="f1637-531">NCC-1701</span></span>                                          |
| <span data-ttu-id="f1637-532">starship:class</span><span class="sxs-lookup"><span data-stu-id="f1637-532">starship:class</span></span>        | <span data-ttu-id="f1637-533">Constitution</span><span class="sxs-lookup"><span data-stu-id="f1637-533">Constitution</span></span>                                      |
| <span data-ttu-id="f1637-534">starship:length</span><span class="sxs-lookup"><span data-stu-id="f1637-534">starship:length</span></span>       | <span data-ttu-id="f1637-535">304.8</span><span class="sxs-lookup"><span data-stu-id="f1637-535">304.8</span></span>                                             |
| <span data-ttu-id="f1637-536">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="f1637-536">starship:commissioned</span></span> | <span data-ttu-id="f1637-537">Falso</span><span class="sxs-lookup"><span data-stu-id="f1637-537">False</span></span>                                             |
| <span data-ttu-id="f1637-538">trademark</span><span class="sxs-lookup"><span data-stu-id="f1637-538">trademark</span></span>             | <span data-ttu-id="f1637-539">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="f1637-539">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="f1637-540">L'app di esempio chiama `GetSection` con la chiave `starship`.</span><span class="sxs-lookup"><span data-stu-id="f1637-540">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="f1637-541">Le coppie chiave-valore `starship` sono isolate.</span><span class="sxs-lookup"><span data-stu-id="f1637-541">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="f1637-542">Il metodo `Bind` viene chiamato sulla sottosezione passando un'istanza della classe `Starship`.</span><span class="sxs-lookup"><span data-stu-id="f1637-542">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="f1637-543">Dopo aver associato i valori dell'istanza, l'istanza viene assegnata a una proprietà per il rendering:</span><span class="sxs-lookup"><span data-stu-id="f1637-543">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="f1637-544">Associazione a un oggetto grafico</span><span class="sxs-lookup"><span data-stu-id="f1637-544">Bind to an object graph</span></span>

<span data-ttu-id="f1637-545"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> è in grado di associare un intero oggetto grafico POCO.</span><span class="sxs-lookup"><span data-stu-id="f1637-545"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="f1637-546">L'esempio contiene un modello `TvShow` il cui oggetto grafico include `Metadata` e le classi `Actors` (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="f1637-546">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f1637-547">L'app di esempio ha un file *tvshow.xml* che contiene i dati di configurazione:</span><span class="sxs-lookup"><span data-stu-id="f1637-547">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="f1637-548">La configurazione viene associata all'intero oggetto grafico `TvShow` con il metodo `Bind`.</span><span class="sxs-lookup"><span data-stu-id="f1637-548">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="f1637-549">L'istanza associata viene assegnata a una proprietà per il rendering:</span><span class="sxs-lookup"><span data-stu-id="f1637-549">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="f1637-550">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) associa e restituisce il tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="f1637-550">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="f1637-551">`Get<T>` può essere più comodo che usare `Bind`.</span><span class="sxs-lookup"><span data-stu-id="f1637-551">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="f1637-552">Il codice seguente mostra come usare `Get<T>` con l'esempio precedente, che consente di assegnare l'istanza associata direttamente alla proprietà usata per il rendering:</span><span class="sxs-lookup"><span data-stu-id="f1637-552">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="f1637-553">Associare una matrice a una classe</span><span class="sxs-lookup"><span data-stu-id="f1637-553">Bind an array to a class</span></span>

<span data-ttu-id="f1637-554">*L'app di esempio dimostra i concetti spiegati in questa sezione.*</span><span class="sxs-lookup"><span data-stu-id="f1637-554">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="f1637-555">Il <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supporta l'associazione di matrici agli oggetti usando gli indici delle matrici nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f1637-555">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="f1637-556">Qualsiasi formato di matrice che espone un segmento di chiave numerico (`:0:`, `:1:`, &hellip; `:{n}:`) è in grado di associare l'array a una matrice di classi POCO.</span><span class="sxs-lookup"><span data-stu-id="f1637-556">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="f1637-557">L'associazione viene fornita per convenzione.</span><span class="sxs-lookup"><span data-stu-id="f1637-557">Binding is provided by convention.</span></span> <span data-ttu-id="f1637-558">I provider di configurazione personalizzati non devono implementare l'associazione di matrici.</span><span class="sxs-lookup"><span data-stu-id="f1637-558">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="f1637-559">**Elaborazione di matrici in memoria**</span><span class="sxs-lookup"><span data-stu-id="f1637-559">**In-memory array processing**</span></span>

<span data-ttu-id="f1637-560">Prendere in considerazione le chiavi di configurazione e i valori indicati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="f1637-560">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="f1637-561">Key</span><span class="sxs-lookup"><span data-stu-id="f1637-561">Key</span></span>             | <span data-ttu-id="f1637-562">Valore</span><span class="sxs-lookup"><span data-stu-id="f1637-562">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="f1637-563">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="f1637-563">array:entries:0</span></span> | <span data-ttu-id="f1637-564">value0</span><span class="sxs-lookup"><span data-stu-id="f1637-564">value0</span></span> |
| <span data-ttu-id="f1637-565">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="f1637-565">array:entries:1</span></span> | <span data-ttu-id="f1637-566">value1</span><span class="sxs-lookup"><span data-stu-id="f1637-566">value1</span></span> |
| <span data-ttu-id="f1637-567">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="f1637-567">array:entries:2</span></span> | <span data-ttu-id="f1637-568">value2</span><span class="sxs-lookup"><span data-stu-id="f1637-568">value2</span></span> |
| <span data-ttu-id="f1637-569">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="f1637-569">array:entries:4</span></span> | <span data-ttu-id="f1637-570">value4</span><span class="sxs-lookup"><span data-stu-id="f1637-570">value4</span></span> |
| <span data-ttu-id="f1637-571">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="f1637-571">array:entries:5</span></span> | <span data-ttu-id="f1637-572">value5</span><span class="sxs-lookup"><span data-stu-id="f1637-572">value5</span></span> |

<span data-ttu-id="f1637-573">Queste chiavi e i valori vengono caricati nell'app di esempio usando il provider di configurazione della memoria:</span><span class="sxs-lookup"><span data-stu-id="f1637-573">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

<span data-ttu-id="f1637-574">La matrice ignora un valore per l'indice &num;3.</span><span class="sxs-lookup"><span data-stu-id="f1637-574">The array skips a value for index &num;3.</span></span> <span data-ttu-id="f1637-575">Lo strumento di associazione di configurazione non è in grado di associare valori null o di creare voci null negli oggetti associati, situazione che diventerà chiara tra poco quando viene illustrato il risultato dell'associazione di questa matrice a un oggetto.</span><span class="sxs-lookup"><span data-stu-id="f1637-575">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="f1637-576">Nell'app di esempio, è disponibile una classe POCO per i dati di configurazione associati:</span><span class="sxs-lookup"><span data-stu-id="f1637-576">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f1637-577">I dati di configurazione sono associati all'oggetto:</span><span class="sxs-lookup"><span data-stu-id="f1637-577">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="f1637-578">È possibile usare anche la sintassi [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*), che produce codice più compatto:</span><span class="sxs-lookup"><span data-stu-id="f1637-578">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="f1637-579">L'oggetto associato, un'istanza di `ArrayExample`, riceve i dati di matrice dalla configurazione.</span><span class="sxs-lookup"><span data-stu-id="f1637-579">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="f1637-580">Indice di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="f1637-580">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="f1637-581">Valore di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="f1637-581">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="f1637-582">0</span><span class="sxs-lookup"><span data-stu-id="f1637-582">0</span></span>                            | <span data-ttu-id="f1637-583">value0</span><span class="sxs-lookup"><span data-stu-id="f1637-583">value0</span></span>                       |
| <span data-ttu-id="f1637-584">1</span><span class="sxs-lookup"><span data-stu-id="f1637-584">1</span></span>                            | <span data-ttu-id="f1637-585">value1</span><span class="sxs-lookup"><span data-stu-id="f1637-585">value1</span></span>                       |
| <span data-ttu-id="f1637-586">2</span><span class="sxs-lookup"><span data-stu-id="f1637-586">2</span></span>                            | <span data-ttu-id="f1637-587">value2</span><span class="sxs-lookup"><span data-stu-id="f1637-587">value2</span></span>                       |
| <span data-ttu-id="f1637-588">3\.</span><span class="sxs-lookup"><span data-stu-id="f1637-588">3</span></span>                            | <span data-ttu-id="f1637-589">value4</span><span class="sxs-lookup"><span data-stu-id="f1637-589">value4</span></span>                       |
| <span data-ttu-id="f1637-590">4</span><span class="sxs-lookup"><span data-stu-id="f1637-590">4</span></span>                            | <span data-ttu-id="f1637-591">value5</span><span class="sxs-lookup"><span data-stu-id="f1637-591">value5</span></span>                       |

<span data-ttu-id="f1637-592">L'indice &num;3 nell'oggetto associato contiene i dati di configurazione per la chiave di configurazione `array:4` e il relativo valore `value4`.</span><span class="sxs-lookup"><span data-stu-id="f1637-592">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="f1637-593">Quando i dati di configurazione contenenti una matrice vengono associati, gli indici di matrice nelle chiavi di configurazione vengono usati semplicemente per scorrere i dati di configurazione quando si crea l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="f1637-593">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="f1637-594">Un valore null non può essere mantenuto nei dati di configurazione e una voce con valore null non viene creata in un oggetto associato quando una matrice nelle chiavi di configurazione ignora uno o più indici.</span><span class="sxs-lookup"><span data-stu-id="f1637-594">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="f1637-595">L'elemento di configurazione mancante per l'indice &num;3 può essere fornito prima dell'associazione all'istanza `ArrayExample` da qualsiasi provider di configurazione che produce la coppia chiave-valore corretta nella configurazione.</span><span class="sxs-lookup"><span data-stu-id="f1637-595">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="f1637-596">Se il codice di esempio include un provider di configurazione JSON aggiuntivo con la coppia chiave-valore mancante, `ArrayExample.Entries` corrisponde alla matrice di configurazione completa:</span><span class="sxs-lookup"><span data-stu-id="f1637-596">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="f1637-597">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="f1637-597">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="f1637-598">In `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="f1637-598">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="f1637-599">La coppia chiave-valore mostrata nella tabella viene caricata nella configurazione.</span><span class="sxs-lookup"><span data-stu-id="f1637-599">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="f1637-600">Key</span><span class="sxs-lookup"><span data-stu-id="f1637-600">Key</span></span>             | <span data-ttu-id="f1637-601">Valore</span><span class="sxs-lookup"><span data-stu-id="f1637-601">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="f1637-602">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="f1637-602">array:entries:3</span></span> | <span data-ttu-id="f1637-603">value3</span><span class="sxs-lookup"><span data-stu-id="f1637-603">value3</span></span> |

<span data-ttu-id="f1637-604">Se l'istanza della classe `ArrayExample` viene associata dopo che il provider di configurazione JSON include la voce per l'indice &num;3, la matrice `ArrayExample.Entries` include il valore.</span><span class="sxs-lookup"><span data-stu-id="f1637-604">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="f1637-605">Indice di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="f1637-605">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="f1637-606">Valore di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="f1637-606">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="f1637-607">0</span><span class="sxs-lookup"><span data-stu-id="f1637-607">0</span></span>                            | <span data-ttu-id="f1637-608">value0</span><span class="sxs-lookup"><span data-stu-id="f1637-608">value0</span></span>                       |
| <span data-ttu-id="f1637-609">1</span><span class="sxs-lookup"><span data-stu-id="f1637-609">1</span></span>                            | <span data-ttu-id="f1637-610">value1</span><span class="sxs-lookup"><span data-stu-id="f1637-610">value1</span></span>                       |
| <span data-ttu-id="f1637-611">2</span><span class="sxs-lookup"><span data-stu-id="f1637-611">2</span></span>                            | <span data-ttu-id="f1637-612">value2</span><span class="sxs-lookup"><span data-stu-id="f1637-612">value2</span></span>                       |
| <span data-ttu-id="f1637-613">3\.</span><span class="sxs-lookup"><span data-stu-id="f1637-613">3</span></span>                            | <span data-ttu-id="f1637-614">value3</span><span class="sxs-lookup"><span data-stu-id="f1637-614">value3</span></span>                       |
| <span data-ttu-id="f1637-615">4</span><span class="sxs-lookup"><span data-stu-id="f1637-615">4</span></span>                            | <span data-ttu-id="f1637-616">value4</span><span class="sxs-lookup"><span data-stu-id="f1637-616">value4</span></span>                       |
| <span data-ttu-id="f1637-617">5</span><span class="sxs-lookup"><span data-stu-id="f1637-617">5</span></span>                            | <span data-ttu-id="f1637-618">value5</span><span class="sxs-lookup"><span data-stu-id="f1637-618">value5</span></span>                       |

<span data-ttu-id="f1637-619">**Elaborazione di matrice JSON**</span><span class="sxs-lookup"><span data-stu-id="f1637-619">**JSON array processing**</span></span>

<span data-ttu-id="f1637-620">Se un file JSON contiene una matrice, vengono create chiavi di configurazione per gli elementi della matrice con un indice di sezione in base zero.</span><span class="sxs-lookup"><span data-stu-id="f1637-620">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="f1637-621">Nel file di configurazione seguente, `subsection` è una matrice:</span><span class="sxs-lookup"><span data-stu-id="f1637-621">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="f1637-622">Il provider di configurazione JSON legge i dati di configurazione nelle coppie chiave-valore seguenti:</span><span class="sxs-lookup"><span data-stu-id="f1637-622">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="f1637-623">Key</span><span class="sxs-lookup"><span data-stu-id="f1637-623">Key</span></span>                     | <span data-ttu-id="f1637-624">Valore</span><span class="sxs-lookup"><span data-stu-id="f1637-624">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="f1637-625">json_array:key</span><span class="sxs-lookup"><span data-stu-id="f1637-625">json_array:key</span></span>          | <span data-ttu-id="f1637-626">valueA</span><span class="sxs-lookup"><span data-stu-id="f1637-626">valueA</span></span> |
| <span data-ttu-id="f1637-627">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="f1637-627">json_array:subsection:0</span></span> | <span data-ttu-id="f1637-628">valueB</span><span class="sxs-lookup"><span data-stu-id="f1637-628">valueB</span></span> |
| <span data-ttu-id="f1637-629">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="f1637-629">json_array:subsection:1</span></span> | <span data-ttu-id="f1637-630">valueC</span><span class="sxs-lookup"><span data-stu-id="f1637-630">valueC</span></span> |
| <span data-ttu-id="f1637-631">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="f1637-631">json_array:subsection:2</span></span> | <span data-ttu-id="f1637-632">valueD</span><span class="sxs-lookup"><span data-stu-id="f1637-632">valueD</span></span> |

<span data-ttu-id="f1637-633">Nell'app di esempio, la classe POCO seguente è disponibile per associare le coppie chiave-valore della configurazione:</span><span class="sxs-lookup"><span data-stu-id="f1637-633">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f1637-634">Dopo l'associazione, `JsonArrayExample.Key` contiene il valore `valueA`.</span><span class="sxs-lookup"><span data-stu-id="f1637-634">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="f1637-635">I valori di sottosezione vengono archiviati nella proprietà matrice POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="f1637-635">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="f1637-636">Indice di `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="f1637-636">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="f1637-637">Valore di `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="f1637-637">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="f1637-638">0</span><span class="sxs-lookup"><span data-stu-id="f1637-638">0</span></span>                                   | <span data-ttu-id="f1637-639">valueB</span><span class="sxs-lookup"><span data-stu-id="f1637-639">valueB</span></span>                              |
| <span data-ttu-id="f1637-640">1</span><span class="sxs-lookup"><span data-stu-id="f1637-640">1</span></span>                                   | <span data-ttu-id="f1637-641">valueC</span><span class="sxs-lookup"><span data-stu-id="f1637-641">valueC</span></span>                              |
| <span data-ttu-id="f1637-642">2</span><span class="sxs-lookup"><span data-stu-id="f1637-642">2</span></span>                                   | <span data-ttu-id="f1637-643">valueD</span><span class="sxs-lookup"><span data-stu-id="f1637-643">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="f1637-644">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="f1637-644">Custom configuration provider</span></span>

<span data-ttu-id="f1637-645">L'app di esempio dimostra come creare un provider di configurazione di base che legge le coppie chiave-valore di configurazione da un database usando [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="f1637-645">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="f1637-646">Il provider ha le caratteristiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="f1637-646">The provider has the following characteristics:</span></span>

* <span data-ttu-id="f1637-647">Il database in memoria di Entity Framework viene usato a scopo dimostrativo.</span><span class="sxs-lookup"><span data-stu-id="f1637-647">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="f1637-648">Per usare un database che richiede una stringa di connessione, implementare un `ConfigurationBuilder` secondario per fornire la stringa di connessione da un altro provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f1637-648">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="f1637-649">Il provider legge una tabella di database in una configurazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="f1637-649">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="f1637-650">Il provider non esegue una query sul database per ogni chiave.</span><span class="sxs-lookup"><span data-stu-id="f1637-650">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="f1637-651">Il ricaricamento in caso di modifica non è implementato, quindi l'aggiornamento del database dopo l'avvio dell'app non ha alcun effetto sulla configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="f1637-651">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="f1637-652">Definire un'entità `EFConfigurationValue` per l'archiviazione dei valori di configurazione nel database.</span><span class="sxs-lookup"><span data-stu-id="f1637-652">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f1637-653">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="f1637-653">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="f1637-654">Aggiungere `EFConfigurationContext` per archiviare i valori configurati e accedervi.</span><span class="sxs-lookup"><span data-stu-id="f1637-654">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="f1637-655">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="f1637-655">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="f1637-656">Creare una classe che implementi <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="f1637-656">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="f1637-657">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="f1637-657">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="f1637-658">Creare il provider di configurazione personalizzato ereditando da <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="f1637-658">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="f1637-659">Il provider di configurazione inizializza il database quando è vuoto.</span><span class="sxs-lookup"><span data-stu-id="f1637-659">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="f1637-660">Poiché le [chiavi di configurazione non fanno distinzione tra maiuscole](#keys)e minuscole, il dizionario utilizzato per inizializzare il database viene creato con l'operatore di confronto senza distinzione tra maiuscole e minuscole ([StringComparer. OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span><span class="sxs-lookup"><span data-stu-id="f1637-660">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="f1637-661">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="f1637-661">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="f1637-662">Un metodo di estensione `AddEFConfiguration` consente di aggiungere l'origine di configurazione a un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f1637-662">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="f1637-663">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="f1637-663">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="f1637-664">L'esempio di codice seguente mostra come usare il `EFConfigurationProvider` personalizzato in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="f1637-664">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f1637-665">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="f1637-665">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="f1637-666">Aggiungere `EFConfigurationContext` per archiviare i valori configurati e accedervi.</span><span class="sxs-lookup"><span data-stu-id="f1637-666">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="f1637-667">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="f1637-667">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="f1637-668">Creare una classe che implementi <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="f1637-668">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="f1637-669">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="f1637-669">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="f1637-670">Creare il provider di configurazione personalizzato ereditando da <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="f1637-670">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="f1637-671">Il provider di configurazione inizializza il database quando è vuoto.</span><span class="sxs-lookup"><span data-stu-id="f1637-671">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="f1637-672">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="f1637-672">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="f1637-673">Un metodo di estensione `AddEFConfiguration` consente di aggiungere l'origine di configurazione a un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f1637-673">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="f1637-674">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="f1637-674">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="f1637-675">L'esempio di codice seguente mostra come usare il `EFConfigurationProvider` personalizzato in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="f1637-675">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="f1637-676">Accedere alla configurazione durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="f1637-676">Access configuration during startup</span></span>

<span data-ttu-id="f1637-677">Inserire `IConfiguration` nel costruttore `Startup` per accedere ai valori di configurazione in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f1637-677">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="f1637-678">Per accedere alla configurazione in `Startup.Configure`, inserire `IConfiguration` direttamente nel metodo o usare l'istanza dal costruttore:</span><span class="sxs-lookup"><span data-stu-id="f1637-678">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="f1637-679">Per un esempio di accesso alla configurazione usando metodi di servizio di avvio, vedere [Avvio dell'applicazione: Metodi pratici](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="f1637-679">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="f1637-680">Accedere alla configurazione in una pagina Razor Pages o in una visualizzazione MVC</span><span class="sxs-lookup"><span data-stu-id="f1637-680">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="f1637-681">Per accedere alle impostazioni di configurazione in una pagina Razor Pages o in una visualizzazione MVC, aggiungere un [direttiva using](xref:mvc/views/razor#using) ([Informazioni di riferimento su C#: direttiva using](/dotnet/csharp/language-reference/keywords/using-directive)) per lo [spazio dei nomi Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) e inserire <xref:Microsoft.Extensions.Configuration.IConfiguration> nella pagina o nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="f1637-681">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="f1637-682">In una pagina Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="f1637-682">In a Razor Pages page:</span></span>

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

<span data-ttu-id="f1637-683">In una visualizzazione MVC:</span><span class="sxs-lookup"><span data-stu-id="f1637-683">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="f1637-684">Aggiungere la configurazione da un assembly esterno</span><span class="sxs-lookup"><span data-stu-id="f1637-684">Add configuration from an external assembly</span></span>

<span data-ttu-id="f1637-685">Un'implementazione <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> consente l'aggiunta di miglioramenti a un'app all'avvio, da un assembly esterno alla classe `Startup` dell'app.</span><span class="sxs-lookup"><span data-stu-id="f1637-685">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="f1637-686">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="f1637-686">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f1637-687">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f1637-687">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
