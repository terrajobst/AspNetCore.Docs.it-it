---
title: Configurazione in ASP.NET Core
author: guardrex
description: Informazioni su come usare l'API di configurazione per configurare un'app ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/11/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 3351ab743ce38b78b1c5857e52020fdeda12cbe7
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/12/2019
ms.locfileid: "67855824"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="dfa44-103">Configurazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dfa44-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="dfa44-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="dfa44-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="dfa44-105">La configurazione delle app in ASP.NET Core si basa su coppie chiave-valore stabilite dai *provider di configurazione*.</span><span class="sxs-lookup"><span data-stu-id="dfa44-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="dfa44-106">I provider di configurazione leggono i dati di configurazione in coppie chiave-valore da un'ampia gamma di origini di configurazione:</span><span class="sxs-lookup"><span data-stu-id="dfa44-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="dfa44-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="dfa44-107">Azure Key Vault</span></span>
* <span data-ttu-id="dfa44-108">Configurazione app di Azure</span><span class="sxs-lookup"><span data-stu-id="dfa44-108">Azure App Configuration</span></span>
* <span data-ttu-id="dfa44-109">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="dfa44-109">Command-line arguments</span></span>
* <span data-ttu-id="dfa44-110">Provider personalizzati (installati o creati)</span><span class="sxs-lookup"><span data-stu-id="dfa44-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="dfa44-111">File della directory</span><span class="sxs-lookup"><span data-stu-id="dfa44-111">Directory files</span></span>
* <span data-ttu-id="dfa44-112">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="dfa44-112">Environment variables</span></span>
* <span data-ttu-id="dfa44-113">Oggetti .NET in memoria</span><span class="sxs-lookup"><span data-stu-id="dfa44-113">In-memory .NET objects</span></span>
* <span data-ttu-id="dfa44-114">File di impostazioni</span><span class="sxs-lookup"><span data-stu-id="dfa44-114">Settings files</span></span>

<span data-ttu-id="dfa44-115">I pacchetti di configurazione per gli scenari di provider di configurazione comuni sono inclusi nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="dfa44-115">Configuration packages for common configuration provider scenarios are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="dfa44-116">Gli esempi di codice che seguono e quelli inclusi nell'app di esempio usano lo spazio dei nomi <xref:Microsoft.Extensions.Configuration>:</span><span class="sxs-lookup"><span data-stu-id="dfa44-116">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="dfa44-117">Il *modello di opzioni* è un'estensione dei concetti di configurazione descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="dfa44-117">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="dfa44-118">Le opzioni usano le classi per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="dfa44-118">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="dfa44-119">Per altre informazioni sull'uso del modello di opzioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-119">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="dfa44-120">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dfa44-120">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="dfa44-121">Host e configurazione delle app</span><span class="sxs-lookup"><span data-stu-id="dfa44-121">Host versus app configuration</span></span>

<span data-ttu-id="dfa44-122">Prima che l'app venga configurata e avviata, viene configurato e avviato un *host*.</span><span class="sxs-lookup"><span data-stu-id="dfa44-122">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="dfa44-123">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="dfa44-123">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="dfa44-124">Sia l'app che l'host vengono configurati tramite i provider di configurazione descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="dfa44-124">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="dfa44-125">Le coppie chiave-valore di configurazione dell'host diventano parte della configurazione globale dell'app.</span><span class="sxs-lookup"><span data-stu-id="dfa44-125">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="dfa44-126">Per altre informazioni su come vengono usati i provider di configurazione per la creazione dell'host e sugli effetti delle origini di configurazione sulla configurazione dell'host, vedere [L'host](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="dfa44-126">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see [The host](xref:fundamentals/index#host).</span></span>

## <a name="default-configuration"></a><span data-ttu-id="dfa44-127">Configurazione predefinita</span><span class="sxs-lookup"><span data-stu-id="dfa44-127">Default configuration</span></span>

<span data-ttu-id="dfa44-128">Le app basate sui modelli [dotnet new](/dotnet/core/tools/dotnet-new) di ASP.NET Core chiamano <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> durante la compilazione di un host.</span><span class="sxs-lookup"><span data-stu-id="dfa44-128">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="dfa44-129"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> fornisce la configurazione predefinita per l'app nell'ordine seguente:</span><span class="sxs-lookup"><span data-stu-id="dfa44-129"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> provides default configuration for the app in the following order:</span></span>

* <span data-ttu-id="dfa44-130">La configurazione dell'host viene specificata da:</span><span class="sxs-lookup"><span data-stu-id="dfa44-130">Host configuration is provided from:</span></span>
  * <span data-ttu-id="dfa44-131">Variabili di ambiente con prefisso `ASPNETCORE_` (ad esempio `ASPNETCORE_ENVIRONMENT`) che usano il [provider di configurazione delle variabili di ambiente](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="dfa44-131">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="dfa44-132">Il prefisso (`ASPNETCORE_`) viene rimosso al caricamento delle coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-132">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="dfa44-133">Argomenti della riga di comando che usano il [provider di configurazione della riga di comando](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="dfa44-133">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="dfa44-134">La configurazione dell'app viene fornita da:</span><span class="sxs-lookup"><span data-stu-id="dfa44-134">App configuration is provided from:</span></span>
  * <span data-ttu-id="dfa44-135">*appsettings.json* mediante il [provider di configurazione dei file](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="dfa44-135">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="dfa44-136">*appsettings.{Ambiente}.json* mediante il [provider di configurazione dei file](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="dfa44-136">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="dfa44-137">[Strumento di gestione dei segreti](xref:security/app-secrets) quando l'app viene eseguita nell'ambiente `Development` usando l'assembly di ingresso.</span><span class="sxs-lookup"><span data-stu-id="dfa44-137">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="dfa44-138">Variabili di ambiente che usano il [provider di configurazione delle variabili di ambiente](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="dfa44-138">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="dfa44-139">Se viene usato un prefisso personalizzato (ad esempio, `PREFIX_` con `.AddEnvironmentVariables(prefix: "PREFIX_")`), il prefisso viene rimosso al caricamento delle coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-139">If a custom prefix is used (for example, `PREFIX_` with `.AddEnvironmentVariables(prefix: "PREFIX_")`), the prefix is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="dfa44-140">Argomenti della riga di comando che usano il [provider di configurazione della riga di comando](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="dfa44-140">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="dfa44-141">I provider di configurazione sono descritti più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="dfa44-141">The configuration providers are explained later in this topic.</span></span> <span data-ttu-id="dfa44-142">Per altre informazioni sull'host e su <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, vedere <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-142">For more information on the host and <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="security"></a><span data-ttu-id="dfa44-143">Sicurezza</span><span class="sxs-lookup"><span data-stu-id="dfa44-143">Security</span></span>

<span data-ttu-id="dfa44-144">Adottare le procedure consigliate seguenti:</span><span class="sxs-lookup"><span data-stu-id="dfa44-144">Adopt the following best practices:</span></span>

* <span data-ttu-id="dfa44-145">Non archiviare mai la password o altri dati sensibili nel codice del provider di configurazione o in file di configurazione di testo normale.</span><span class="sxs-lookup"><span data-stu-id="dfa44-145">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="dfa44-146">Non usare i segreti di produzione in ambienti di sviluppo o di test.</span><span class="sxs-lookup"><span data-stu-id="dfa44-146">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="dfa44-147">Specificare i segreti all'esterno del progetto in modo che non possano essere inavvertitamente inviati a un repository del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="dfa44-147">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="dfa44-148">Altre informazioni su [come usare più ambienti](xref:fundamentals/environments) e la gestione dell'[archiviazione sicura dei segreti delle app durante lo sviluppo con Secret Manager](xref:security/app-secrets) (include consigli sull'uso delle variabili di ambiente per l'archiviazione di dati sensibili).</span><span class="sxs-lookup"><span data-stu-id="dfa44-148">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="dfa44-149">Secret Manager usa il provider di configurazione dei file per archiviare i segreti utente in un file JSON nel sistema locale.</span><span class="sxs-lookup"><span data-stu-id="dfa44-149">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="dfa44-150">Il provider di configurazione dei file è descritto più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="dfa44-150">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="dfa44-151">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) è una delle opzioni per l'archiviazione sicura dei segreti delle app.</span><span class="sxs-lookup"><span data-stu-id="dfa44-151">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="dfa44-152">Per altre informazioni, vedere <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-152">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="dfa44-153">Dati di configurazione gerarchici</span><span class="sxs-lookup"><span data-stu-id="dfa44-153">Hierarchical configuration data</span></span>

<span data-ttu-id="dfa44-154">L'API di configurazione è in grado di mantenere i dati di configurazione gerarchici rendendoli flat grazie all'uso di un delimitatore nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-154">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="dfa44-155">Nel file JSON seguente, esistono quattro chiavi in una gerarchia strutturata di due sezioni:</span><span class="sxs-lookup"><span data-stu-id="dfa44-155">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="dfa44-156">Quando il file viene letto nella configurazione, vengono create chiavi univoche per mantenere la struttura di dati gerarchici originale dell'origine di configurazione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-156">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="dfa44-157">Le sezioni e le chiavi vengono rese flat usando due punti (`:`) per mantenere la struttura originale:</span><span class="sxs-lookup"><span data-stu-id="dfa44-157">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="dfa44-158">section0:key0</span><span class="sxs-lookup"><span data-stu-id="dfa44-158">section0:key0</span></span>
* <span data-ttu-id="dfa44-159">section0:key1</span><span class="sxs-lookup"><span data-stu-id="dfa44-159">section0:key1</span></span>
* <span data-ttu-id="dfa44-160">section1:key0</span><span class="sxs-lookup"><span data-stu-id="dfa44-160">section1:key0</span></span>
* <span data-ttu-id="dfa44-161">section1:key1</span><span class="sxs-lookup"><span data-stu-id="dfa44-161">section1:key1</span></span>

<span data-ttu-id="dfa44-162">I metodi <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> e <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> sono disponibili per isolare le sezioni e gli elementi figlio di una sezione nei dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-162"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="dfa44-163">Questi metodi sono descritti più avanti in [GetSection, GetChildren ed Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="dfa44-163">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span> <span data-ttu-id="dfa44-164">`GetSection` è incluso nel pacchetto [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="dfa44-164">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="conventions"></a><span data-ttu-id="dfa44-165">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="dfa44-165">Conventions</span></span>

<span data-ttu-id="dfa44-166">All'avvio dell'app, le origini di configurazione vengono lette nell'ordine con cui sono specificati i rispettivi provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-166">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="dfa44-167">I provider di configurazione che implementano il rilevamento delle modifiche sono in grado di ricaricare la configurazione quando viene modificata un'impostazione sottostante.</span><span class="sxs-lookup"><span data-stu-id="dfa44-167">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="dfa44-168">Ad esempio, il provider di configurazione dei file (descritto più avanti in questo argomento) e il [provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration) implementano il rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="dfa44-168">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="dfa44-169"><xref:Microsoft.Extensions.Configuration.IConfiguration> è disponibile nel contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) dell'app.</span><span class="sxs-lookup"><span data-stu-id="dfa44-169"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="dfa44-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> può essere inserito in <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> di Razor Pages per ottenere la configurazione per la classe:</span><span class="sxs-lookup"><span data-stu-id="dfa44-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

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

<span data-ttu-id="dfa44-171">I provider di configurazione non possono usare l'inserimento delle dipendenze, perché non è disponibile quando vengono configurati dall'host.</span><span class="sxs-lookup"><span data-stu-id="dfa44-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="dfa44-172">Le chiavi di configurazione adottano le convenzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="dfa44-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="dfa44-173">Per le chiavi non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="dfa44-173">Keys are case-insensitive.</span></span> <span data-ttu-id="dfa44-174">Ad esempio, `ConnectionString` e `connectionstring` vengono considerate chiavi equivalenti.</span><span class="sxs-lookup"><span data-stu-id="dfa44-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="dfa44-175">Se un valore per la stessa chiave viene impostato dallo stesso provider di configurazione o da provider diversi, viene usato l'ultimo valore impostato per la chiave.</span><span class="sxs-lookup"><span data-stu-id="dfa44-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="dfa44-176">Chiavi gerarchiche</span><span class="sxs-lookup"><span data-stu-id="dfa44-176">Hierarchical keys</span></span>
  * <span data-ttu-id="dfa44-177">Nell'ambito dell'API di configurazione, il separatore due punti (`:`) funziona in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="dfa44-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="dfa44-178">Nelle variabili di ambiente, un separatore due punti potrebbe non funzionare in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="dfa44-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="dfa44-179">Il doppio carattere di sottolineatura (`__`) è supportato da tutte le piattaforme e viene convertito in due punti.</span><span class="sxs-lookup"><span data-stu-id="dfa44-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="dfa44-180">In Azure Key Vault, le chiavi gerarchiche usano `--` (due trattini) come separatore.</span><span class="sxs-lookup"><span data-stu-id="dfa44-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="dfa44-181">È necessario fornire il codice per sostituire i trattini con due punti quando i segreti vengono caricati nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="dfa44-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="dfa44-182">Il <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supporta l'associazione di matrici agli oggetti usando gli indici delle matrici nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="dfa44-183">L'associazione di matrici è descritta nella sezione [Associare una matrice a una classe](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="dfa44-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="dfa44-184">I valori di configurazione adottano le convenzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="dfa44-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="dfa44-185">I valori sono stringhe.</span><span class="sxs-lookup"><span data-stu-id="dfa44-185">Values are strings.</span></span>
* <span data-ttu-id="dfa44-186">I valori null non possono essere archiviati nella configurazione o associati a oggetti.</span><span class="sxs-lookup"><span data-stu-id="dfa44-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="dfa44-187">Provider</span><span class="sxs-lookup"><span data-stu-id="dfa44-187">Providers</span></span>

<span data-ttu-id="dfa44-188">La tabella seguente mostra i provider di configurazione disponibili per le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dfa44-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="dfa44-189">Provider</span><span class="sxs-lookup"><span data-stu-id="dfa44-189">Provider</span></span> | <span data-ttu-id="dfa44-190">Fornisce la configurazione da&hellip;</span><span class="sxs-lookup"><span data-stu-id="dfa44-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="dfa44-191">[Provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="dfa44-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="dfa44-192">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="dfa44-192">Azure Key Vault</span></span> |
| <span data-ttu-id="dfa44-193">[Provider di Configurazione app](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Documentazione di Azure)</span><span class="sxs-lookup"><span data-stu-id="dfa44-193">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="dfa44-194">Configurazione app di Azure</span><span class="sxs-lookup"><span data-stu-id="dfa44-194">Azure App Configuration</span></span> |
| [<span data-ttu-id="dfa44-195">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="dfa44-195">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="dfa44-196">Parametri della riga di comando</span><span class="sxs-lookup"><span data-stu-id="dfa44-196">Command-line parameters</span></span> |
| [<span data-ttu-id="dfa44-197">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="dfa44-197">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="dfa44-198">Origine personalizzata</span><span class="sxs-lookup"><span data-stu-id="dfa44-198">Custom source</span></span> |
| [<span data-ttu-id="dfa44-199">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="dfa44-199">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="dfa44-200">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="dfa44-200">Environment variables</span></span> |
| [<span data-ttu-id="dfa44-201">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="dfa44-201">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="dfa44-202">File (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="dfa44-202">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="dfa44-203">Provider di configurazione chiave-per-file</span><span class="sxs-lookup"><span data-stu-id="dfa44-203">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="dfa44-204">File della directory</span><span class="sxs-lookup"><span data-stu-id="dfa44-204">Directory files</span></span> |
| [<span data-ttu-id="dfa44-205">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="dfa44-205">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="dfa44-206">Raccolte in memoria</span><span class="sxs-lookup"><span data-stu-id="dfa44-206">In-memory collections</span></span> |
| <span data-ttu-id="dfa44-207">[Segreti utente (Secret Manager)](xref:security/app-secrets) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="dfa44-207">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="dfa44-208">File nella directory dei profili utente</span><span class="sxs-lookup"><span data-stu-id="dfa44-208">File in the user profile directory</span></span> |

<span data-ttu-id="dfa44-209">Le origini di configurazione vengono lette nell'ordine in cui vengono specificati i rispetti provider di configurazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="dfa44-209">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="dfa44-210">I provider di configurazione descritti in questo argomento vengono presentati in ordine alfabetico e non nell'ordine in cui potrebbe disporli il codice.</span><span class="sxs-lookup"><span data-stu-id="dfa44-210">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="dfa44-211">Ordinare i provider di configurazione nel codice in base alle specifiche priorità per le origini di configurazione sottostanti.</span><span class="sxs-lookup"><span data-stu-id="dfa44-211">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="dfa44-212">Una sequenza tipica di provider di configurazione è:</span><span class="sxs-lookup"><span data-stu-id="dfa44-212">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="dfa44-213">File (*appsettings.json*, *appsettings.{Environment}.json*, dove `{Environment}` è l'ambiente di hosting corrente dell'app)</span><span class="sxs-lookup"><span data-stu-id="dfa44-213">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="dfa44-214">Insieme di credenziali delle chiavi di Azure</span><span class="sxs-lookup"><span data-stu-id="dfa44-214">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="dfa44-215">[Segreti utente (Secret Manager)](xref:security/app-secrets) (sono nell'ambiente Development)</span><span class="sxs-lookup"><span data-stu-id="dfa44-215">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="dfa44-216">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="dfa44-216">Environment variables</span></span>
1. <span data-ttu-id="dfa44-217">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="dfa44-217">Command-line arguments</span></span>

<span data-ttu-id="dfa44-218">È pratica comune posizionare il provider di configurazione della riga di comando per ultimo in una serie di provider per consentire agli argomenti della riga di comando di sostituire la configurazione impostata da altri provider.</span><span class="sxs-lookup"><span data-stu-id="dfa44-218">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="dfa44-219">Questa sequenza di provider viene applicata quando si inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-219">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="dfa44-220">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="dfa44-220">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

## <a name="configureappconfiguration"></a><span data-ttu-id="dfa44-221">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="dfa44-221">ConfigureAppConfiguration</span></span>

<span data-ttu-id="dfa44-222">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare i provider di configurazione dell'app, oltre a quelli aggiunti automaticamente da <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="dfa44-222">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

<span data-ttu-id="dfa44-223">La configurazione fornita all'app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> è disponibile durante l'avvio dell'app, incluso `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-223">Configuration supplied to the app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="dfa44-224">Per altre informazioni, vedere la sezione [Accedere alla configurazione durante l'avvio](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="dfa44-224">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="dfa44-225">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="dfa44-225">Command-line Configuration Provider</span></span>

<span data-ttu-id="dfa44-226"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> carica la configurazione da coppie chiave-valore di argomenti della riga di comando in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-226">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="dfa44-227">Per attivare la configurazione della riga di comando, metodo di estensione <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> viene chiamato su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-227">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="dfa44-228">`AddCommandLine` viene chiamato automaticamente quando si Inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-228">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="dfa44-229">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="dfa44-229">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="dfa44-230">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="dfa44-230">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="dfa44-231">Una configurazione facoltativa da *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="dfa44-231">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="dfa44-232">[Segreti utente (Secret Manager)](xref:security/app-secrets) (nell'ambiente Development).</span><span class="sxs-lookup"><span data-stu-id="dfa44-232">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="dfa44-233">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="dfa44-233">Environment variables.</span></span>

<span data-ttu-id="dfa44-234">`CreateDefaultBuilder` aggiunge il provider di configurazione della riga di comando per ultimo.</span><span class="sxs-lookup"><span data-stu-id="dfa44-234">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="dfa44-235">Gli argomenti della riga di comando passati in fase di esecuzione sostituiscono la configurazione impostata dagli altri provider.</span><span class="sxs-lookup"><span data-stu-id="dfa44-235">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="dfa44-236">`CreateDefaultBuilder` opera durante la creazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="dfa44-236">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="dfa44-237">Pertanto, la configurazione della riga di comando attivata da `CreateDefaultBuilder` può influire sul modo in cui l'host viene configurato.</span><span class="sxs-lookup"><span data-stu-id="dfa44-237">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="dfa44-238">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="dfa44-238">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="dfa44-239">`AddCommandLine` è già stato chiamato da `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-239">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="dfa44-240">Se è necessario specificare la configurazione dell'app ed è ancora possibile eseguire l'override della configurazione con argomenti della riga di comando, chiamare i provider aggiuntivi dell'app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> e chiamare infine `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-240">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call other providers here and call AddCommandLine last.
                config.AddCommandLine(args);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="dfa44-241">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="dfa44-241">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var config = new ConfigurationBuilder()
    // Call additional providers here as needed.
    // Call AddCommandLine last to allow arguments to override other configuration.
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="dfa44-242">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="dfa44-242">**Example**</span></span>

<span data-ttu-id="dfa44-243">L'app di esempio consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include una chiamata a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-243">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="dfa44-244">Aprire un prompt dei comandi nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="dfa44-244">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="dfa44-245">Fornire un argomento della riga di comando per il comando `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-245">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="dfa44-246">Dopo che l'app è in esecuzione, aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-246">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="dfa44-247">Notare che l'output contiene la coppia chiave-valore per l'argomento della riga di comando di configurazione fornito per `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-247">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="dfa44-248">Argomenti</span><span class="sxs-lookup"><span data-stu-id="dfa44-248">Arguments</span></span>

<span data-ttu-id="dfa44-249">Il valore deve seguire un segno di uguale (`=`) o la chiave deve avere un prefisso (`--` o `/`) quando il valore segue uno spazio.</span><span class="sxs-lookup"><span data-stu-id="dfa44-249">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="dfa44-250">Il valore può essere null se viene usato un segno di uguale (ad esempio, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="dfa44-250">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="dfa44-251">Prefisso chiave</span><span class="sxs-lookup"><span data-stu-id="dfa44-251">Key prefix</span></span>               | <span data-ttu-id="dfa44-252">Esempio</span><span class="sxs-lookup"><span data-stu-id="dfa44-252">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="dfa44-253">Nessun prefisso</span><span class="sxs-lookup"><span data-stu-id="dfa44-253">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="dfa44-254">Due trattini (`--`)</span><span class="sxs-lookup"><span data-stu-id="dfa44-254">Two dashes (`--`)</span></span>        | <span data-ttu-id="dfa44-255">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="dfa44-255">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="dfa44-256">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="dfa44-256">Forward slash (`/`)</span></span>      | <span data-ttu-id="dfa44-257">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="dfa44-257">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="dfa44-258">Nello stesso comando, non mischiare coppie chiave-valore di argomenti della riga di comando che usano un segno di uguale con coppie chiave-valore che usano uno spazio.</span><span class="sxs-lookup"><span data-stu-id="dfa44-258">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="dfa44-259">Comandi di esempio:</span><span class="sxs-lookup"><span data-stu-id="dfa44-259">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="dfa44-260">Mapping di sostituzione</span><span class="sxs-lookup"><span data-stu-id="dfa44-260">Switch mappings</span></span>

<span data-ttu-id="dfa44-261">I mapping di sostituzione consentono di usare la logica di sostituzione del nome della chiave.</span><span class="sxs-lookup"><span data-stu-id="dfa44-261">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="dfa44-262">Quando si crea manualmente la configurazione con un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, è possibile specificare un dizionario di mapping di sostituzione per il metodo <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-262">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="dfa44-263">Quando viene utilizzato il dizionario dei mapping di sostituzione, nel dizionario viene controllata la presenza di una chiave corrispondente alla chiave fornita da un argomento della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="dfa44-263">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="dfa44-264">Se la chiave della riga di comando viene trovata nel dizionario, il valore del dizionario (la sostituzione della chiave) viene passato nuovamente per impostare la coppia chiave-valore nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="dfa44-264">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="dfa44-265">Un mapping di sostituzione è necessario per le chiavi della riga di comando con un trattino singolo (`-`) come prefisso.</span><span class="sxs-lookup"><span data-stu-id="dfa44-265">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="dfa44-266">Regole principali del dizionario dei mapping di sostituzione:</span><span class="sxs-lookup"><span data-stu-id="dfa44-266">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="dfa44-267">Le sostituzioni devono iniziare con un trattino (`-`) o un trattino doppio (`--`).</span><span class="sxs-lookup"><span data-stu-id="dfa44-267">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="dfa44-268">Il dizionario dei mapping di sostituzione non deve contenere chiavi duplicate.</span><span class="sxs-lookup"><span data-stu-id="dfa44-268">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="dfa44-269">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="dfa44-269">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _switchMappings = 
        new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    // Do not pass the args to CreateDefaultBuilder
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder()
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddCommandLine(args, _switchMappings);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="dfa44-270">Come illustrato nell'esempio precedente, la chiamata a `CreateDefaultBuilder` non deve passare argomenti quando vengono usati i mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-270">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="dfa44-271">La chiamata `AddCommandLine` del metodo `CreateDefaultBuilder` non include sostituzioni mappate e non è possibile passare il dizionario dei mapping di sostituzione a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-271">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="dfa44-272">Se gli argomenti includono una sostituzione mappata e vengono passati a `CreateDefaultBuilder`, l'inizializzazione del rispettivo provider `AddCommandLine` non riesce con <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-272">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="dfa44-273">La soluzione consiste nel non passare gli argomenti a `CreateDefaultBuilder` consentendo invece al metodo `AddCommandLine` del metodo `ConfigurationBuilder` di elaborare entrambi gli argomenti e il dizionario dei mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-273">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="dfa44-274">Il dizionario dei mapping di sostituzione creato contiene i dati visualizzati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="dfa44-274">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="dfa44-275">Chiave</span><span class="sxs-lookup"><span data-stu-id="dfa44-275">Key</span></span>       | <span data-ttu-id="dfa44-276">Value</span><span class="sxs-lookup"><span data-stu-id="dfa44-276">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="dfa44-277">Se le chiavi con mapping di sostituzione vengono usate all'avvio dell'app, la configurazione riceve il valore di configurazione per la chiave fornita dal dizionario:</span><span class="sxs-lookup"><span data-stu-id="dfa44-277">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="dfa44-278">Dopo aver eseguito il comando precedente, la configurazione contiene i valori mostrati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="dfa44-278">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="dfa44-279">Chiave</span><span class="sxs-lookup"><span data-stu-id="dfa44-279">Key</span></span>               | <span data-ttu-id="dfa44-280">Value</span><span class="sxs-lookup"><span data-stu-id="dfa44-280">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="dfa44-281">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="dfa44-281">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="dfa44-282"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> carica la configurazione da coppie chiave-valore di variabili di ambiente in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-282">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="dfa44-283">Per attivare la configurazione delle variabili di ambiente, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-283">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="dfa44-284">[Servizio App di Azure](https://azure.microsoft.com/services/app-service/) consente di impostare variabili di ambiente nel portale di Azure che possono sostituire la configurazione delle app tramite il provider di configurazione delle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="dfa44-284">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="dfa44-285">Per altre informazioni, vedere [App Azure: Eseguire l'override della configurazione delle app usando il portale di Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="dfa44-285">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="dfa44-286">`AddEnvironmentVariables` consente di caricare variabili di ambiente con prefisso `ASPNETCORE_` per la [configurazione host](#host-versus-app-configuration) quando viene inizializzato un nuovo elemento <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-286">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> is initialized.</span></span> <span data-ttu-id="dfa44-287">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="dfa44-287">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="dfa44-288">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="dfa44-288">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="dfa44-289">Configurazione delle app dalle variabili di ambiente senza prefisso chiamando `AddEnvironmentVariables` senza prefisso.</span><span class="sxs-lookup"><span data-stu-id="dfa44-289">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="dfa44-290">Una configurazione facoltativa da *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="dfa44-290">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="dfa44-291">[Segreti utente (Secret Manager)](xref:security/app-secrets) (nell'ambiente Development).</span><span class="sxs-lookup"><span data-stu-id="dfa44-291">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="dfa44-292">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="dfa44-292">Command-line arguments.</span></span>

<span data-ttu-id="dfa44-293">Il provider di configurazione delle variabili di ambiente viene chiamato dopo aver stabilito la configurazione dai segreti utente e dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="dfa44-293">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="dfa44-294">La chiamata del provider in questa posizione consente alle variabili di ambiente lette in fase di esecuzione di sostituire la configurazione impostata dai segreti utente e dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="dfa44-294">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="dfa44-295">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="dfa44-295">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="dfa44-296">Se è necessario specificare la configurazione dell'app da variabili di ambiente aggiuntive, chiamare i provider aggiuntivi dell'app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> e chiamare `AddEnvironmentVariables` con il prefisso.</span><span class="sxs-lookup"><span data-stu-id="dfa44-296">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call additional providers here as needed.
                // Call AddEnvironmentVariables last if you need to allow
                // environment variables to override values from other 
                // providers.
                config.AddEnvironmentVariables(prefix: "PREFIX_");
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="dfa44-297">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="dfa44-297">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="dfa44-298">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="dfa44-298">**Example**</span></span>

<span data-ttu-id="dfa44-299">L'app di esempio consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include una chiamata a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-299">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="dfa44-300">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="dfa44-300">Run the sample app.</span></span> <span data-ttu-id="dfa44-301">Aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-301">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="dfa44-302">Notare che l'output contiene la coppia chiave-valore per la variabile di ambiente `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-302">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="dfa44-303">Il valore riflette l'ambiente in cui l'app è in esecuzione, in genere `Development` durante l'esecuzione locale.</span><span class="sxs-lookup"><span data-stu-id="dfa44-303">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="dfa44-304">Per mantenere breve l'elenco delle variabili di ambiente restituito dall'app, l'app filtra le variabili di ambiente includendo quelle che iniziano come segue:</span><span class="sxs-lookup"><span data-stu-id="dfa44-304">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="dfa44-305">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="dfa44-305">ASPNETCORE_</span></span>
* <span data-ttu-id="dfa44-306">urls</span><span class="sxs-lookup"><span data-stu-id="dfa44-306">urls</span></span>
* <span data-ttu-id="dfa44-307">Registrazione</span><span class="sxs-lookup"><span data-stu-id="dfa44-307">Logging</span></span>
* <span data-ttu-id="dfa44-308">ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="dfa44-308">ENVIRONMENT</span></span>
* <span data-ttu-id="dfa44-309">contentRoot</span><span class="sxs-lookup"><span data-stu-id="dfa44-309">contentRoot</span></span>
* <span data-ttu-id="dfa44-310">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="dfa44-310">AllowedHosts</span></span>
* <span data-ttu-id="dfa44-311">applicationName</span><span class="sxs-lookup"><span data-stu-id="dfa44-311">applicationName</span></span>
* <span data-ttu-id="dfa44-312">CommandLine</span><span class="sxs-lookup"><span data-stu-id="dfa44-312">CommandLine</span></span>

<span data-ttu-id="dfa44-313">Se si desidera esporre tutte le variabili di ambiente disponibili all'app, modificare `FilteredConfiguration` in *Pages/Index.cshtml.cs* come segue:</span><span class="sxs-lookup"><span data-stu-id="dfa44-313">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="dfa44-314">Prefissi</span><span class="sxs-lookup"><span data-stu-id="dfa44-314">Prefixes</span></span>

<span data-ttu-id="dfa44-315">Le variabili di ambiente caricate nella configurazione dell'app vengono filtrate quando si specifica un prefisso per il metodo `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-315">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="dfa44-316">Ad esempio, per filtrare le variabili di ambiente in base al prefisso `CUSTOM_`, fornire il prefisso al provider di configurazione:</span><span class="sxs-lookup"><span data-stu-id="dfa44-316">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="dfa44-317">Il prefisso viene rimosso quando vengono create le coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-317">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="dfa44-318">Il metodo di servizio statico `CreateDefaultBuilder` crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> per stabilire l'host dell'app.</span><span class="sxs-lookup"><span data-stu-id="dfa44-318">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="dfa44-319">Quando viene creato `WebHostBuilder`, la configurazione dell'host corrispondente viene individuata nelle variabili di ambiente con il prefisso `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-319">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

<span data-ttu-id="dfa44-320">**Prefissi della stringa di connessione**</span><span class="sxs-lookup"><span data-stu-id="dfa44-320">**Connection string prefixes**</span></span>

<span data-ttu-id="dfa44-321">L'API di configurazione prevede regole di elaborazione speciali per quattro variabili di ambiente di stringa di connessione coinvolte nella configurazione di stringhe di connessione di Azure per l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="dfa44-321">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="dfa44-322">Le variabili di ambiente con i prefissi indicati nella tabella vengono caricate nell'app se non viene fornito alcun prefisso a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-322">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="dfa44-323">Prefisso della stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="dfa44-323">Connection string prefix</span></span> | <span data-ttu-id="dfa44-324">Provider</span><span class="sxs-lookup"><span data-stu-id="dfa44-324">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="dfa44-325">Provider personalizzato</span><span class="sxs-lookup"><span data-stu-id="dfa44-325">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="dfa44-326">MySQL</span><span class="sxs-lookup"><span data-stu-id="dfa44-326">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="dfa44-327">Database SQL di Azure</span><span class="sxs-lookup"><span data-stu-id="dfa44-327">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="dfa44-328">SQL Server</span><span class="sxs-lookup"><span data-stu-id="dfa44-328">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="dfa44-329">Quando una variabile di ambiente viene individuata e caricata nella configurazione con uno qualsiasi dei quattro prefissi indicati nella tabella:</span><span class="sxs-lookup"><span data-stu-id="dfa44-329">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="dfa44-330">La chiave di configurazione viene creata rimuovendo il prefisso della variabile di ambiente e aggiungendo una sezione per la chiave di configurazione (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="dfa44-330">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="dfa44-331">Viene creata una nuova coppia chiave-valore della configurazione che rappresenta il provider di connessione al database (ad eccezione di `CUSTOMCONNSTR_`, che non ha un provider dichiarato).</span><span class="sxs-lookup"><span data-stu-id="dfa44-331">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="dfa44-332">Chiave della variabile di ambiente</span><span class="sxs-lookup"><span data-stu-id="dfa44-332">Environment variable key</span></span> | <span data-ttu-id="dfa44-333">Chiave di configurazione convertita</span><span class="sxs-lookup"><span data-stu-id="dfa44-333">Converted configuration key</span></span> | <span data-ttu-id="dfa44-334">Voce di configurazione del provider</span><span class="sxs-lookup"><span data-stu-id="dfa44-334">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="dfa44-335">Voce di configurazione non creata.</span><span class="sxs-lookup"><span data-stu-id="dfa44-335">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="dfa44-336">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="dfa44-336">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="dfa44-337">Valore: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="dfa44-337">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="dfa44-338">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="dfa44-338">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="dfa44-339">Valore: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="dfa44-339">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="dfa44-340">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="dfa44-340">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="dfa44-341">Valore: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="dfa44-341">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="dfa44-342">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="dfa44-342">File Configuration Provider</span></span>

<span data-ttu-id="dfa44-343"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> è la classe base per il caricamento della configurazione dal file system.</span><span class="sxs-lookup"><span data-stu-id="dfa44-343"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="dfa44-344">I provider di configurazione seguenti sono dedicati per specifici tipi di file:</span><span class="sxs-lookup"><span data-stu-id="dfa44-344">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="dfa44-345">Provider di configurazione INI</span><span class="sxs-lookup"><span data-stu-id="dfa44-345">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="dfa44-346">Provider di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="dfa44-346">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="dfa44-347">Provider di configurazione XML</span><span class="sxs-lookup"><span data-stu-id="dfa44-347">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="dfa44-348">Provider di configurazione INI</span><span class="sxs-lookup"><span data-stu-id="dfa44-348">INI Configuration Provider</span></span>

<span data-ttu-id="dfa44-349"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> carica la configurazione da coppie chiave-valore di file INI in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-349">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="dfa44-350">Per attivare la configurazione di file INI, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-350">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="dfa44-351">I due punti possono essere usati come delimitatore di sezione nella configurazione di file INI.</span><span class="sxs-lookup"><span data-stu-id="dfa44-351">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="dfa44-352">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="dfa44-352">Overloads permit specifying:</span></span>

* <span data-ttu-id="dfa44-353">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="dfa44-353">Whether the file is optional.</span></span>
* <span data-ttu-id="dfa44-354">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="dfa44-354">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="dfa44-355">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="dfa44-355">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="dfa44-356">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="dfa44-356">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddIniFile(
                    "config.ini", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="dfa44-357">Il percorso di base è impostato con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-357">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="dfa44-358">`SetBasePath` è incluso nel pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="dfa44-358">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="dfa44-359">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="dfa44-359">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddIniFile("config.ini", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="dfa44-360">Il percorso di base è impostato con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-360">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="dfa44-361">`SetBasePath` è incluso nel pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="dfa44-361">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="dfa44-362">Un esempio generico di un file di configurazione INI:</span><span class="sxs-lookup"><span data-stu-id="dfa44-362">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="dfa44-363">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="dfa44-363">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="dfa44-364">section0:key0</span><span class="sxs-lookup"><span data-stu-id="dfa44-364">section0:key0</span></span>
* <span data-ttu-id="dfa44-365">section0:key1</span><span class="sxs-lookup"><span data-stu-id="dfa44-365">section0:key1</span></span>
* <span data-ttu-id="dfa44-366">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="dfa44-366">section1:subsection:key</span></span>
* <span data-ttu-id="dfa44-367">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="dfa44-367">section2:subsection0:key</span></span>
* <span data-ttu-id="dfa44-368">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="dfa44-368">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="dfa44-369">Provider di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="dfa44-369">JSON Configuration Provider</span></span>

<span data-ttu-id="dfa44-370">Il <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> carica la configurazione da coppie chiave-valore di file JSON in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-370">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="dfa44-371">Per attivare la configurazione di file JSON, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-371">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="dfa44-372">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="dfa44-372">Overloads permit specifying:</span></span>

* <span data-ttu-id="dfa44-373">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="dfa44-373">Whether the file is optional.</span></span>
* <span data-ttu-id="dfa44-374">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="dfa44-374">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="dfa44-375">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="dfa44-375">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="dfa44-376">`AddJsonFile` viene chiamato automaticamente due volte quando si inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-376">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="dfa44-377">Il metodo viene chiamato per caricare la configurazione da:</span><span class="sxs-lookup"><span data-stu-id="dfa44-377">The method is called to load configuration from:</span></span>

* <span data-ttu-id="dfa44-378">*appsettings.json* &ndash; Questo file viene letto per primo.</span><span class="sxs-lookup"><span data-stu-id="dfa44-378">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="dfa44-379">La versione dell'ambiente del file può sostituire i valori forniti dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="dfa44-379">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="dfa44-380">*appsettings.{Environment}.json* &ndash; La versione dell'ambiente del file viene caricata in base a [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="dfa44-380">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="dfa44-381">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="dfa44-381">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="dfa44-382">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="dfa44-382">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="dfa44-383">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="dfa44-383">Environment variables.</span></span>
* <span data-ttu-id="dfa44-384">[Segreti utente (Secret Manager)](xref:security/app-secrets) (nell'ambiente Development).</span><span class="sxs-lookup"><span data-stu-id="dfa44-384">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="dfa44-385">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="dfa44-385">Command-line arguments.</span></span>

<span data-ttu-id="dfa44-386">Il provider di configurazione JSON viene stabilito per primo.</span><span class="sxs-lookup"><span data-stu-id="dfa44-386">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="dfa44-387">I segreti utente, le variabili di ambiente e gli argomenti della riga di comando sostituiscono quindi la configurazione impostata dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="dfa44-387">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="dfa44-388">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app per file diversi da *appsettings.json* e *appsettings.{Environment}.json*:</span><span class="sxs-lookup"><span data-stu-id="dfa44-388">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddJsonFile(
                    "config.json", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="dfa44-389">Il percorso di base è impostato con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-389">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="dfa44-390">`SetBasePath` è incluso nel pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="dfa44-390">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="dfa44-391">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="dfa44-391">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("config.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="dfa44-392">Il percorso di base è impostato con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-392">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="dfa44-393">`SetBasePath` è incluso nel pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="dfa44-393">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="dfa44-394">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="dfa44-394">**Example**</span></span>

<span data-ttu-id="dfa44-395">L'app di esempio consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include due chiamate a `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-395">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="dfa44-396">La configurazione viene caricata da *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="dfa44-396">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

1. <span data-ttu-id="dfa44-397">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="dfa44-397">Run the sample app.</span></span> <span data-ttu-id="dfa44-398">Aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-398">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="dfa44-399">Notare che l'output contiene coppie chiave-valore per la configurazione mostrata nella tabella in base all'ambiente.</span><span class="sxs-lookup"><span data-stu-id="dfa44-399">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="dfa44-400">Le chiavi di configurazione di registrazione usano i due punti (`:`) come separatore gerarchico.</span><span class="sxs-lookup"><span data-stu-id="dfa44-400">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="dfa44-401">Chiave</span><span class="sxs-lookup"><span data-stu-id="dfa44-401">Key</span></span>                        | <span data-ttu-id="dfa44-402">Valore Development</span><span class="sxs-lookup"><span data-stu-id="dfa44-402">Development Value</span></span> | <span data-ttu-id="dfa44-403">Valore Production</span><span class="sxs-lookup"><span data-stu-id="dfa44-403">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="dfa44-404">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="dfa44-404">Logging:LogLevel:System</span></span>    | <span data-ttu-id="dfa44-405">Informazioni</span><span class="sxs-lookup"><span data-stu-id="dfa44-405">Information</span></span>       | <span data-ttu-id="dfa44-406">Informazioni</span><span class="sxs-lookup"><span data-stu-id="dfa44-406">Information</span></span>      |
| <span data-ttu-id="dfa44-407">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="dfa44-407">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="dfa44-408">Informazioni</span><span class="sxs-lookup"><span data-stu-id="dfa44-408">Information</span></span>       | <span data-ttu-id="dfa44-409">Informazioni</span><span class="sxs-lookup"><span data-stu-id="dfa44-409">Information</span></span>      |
| <span data-ttu-id="dfa44-410">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="dfa44-410">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="dfa44-411">Debug</span><span class="sxs-lookup"><span data-stu-id="dfa44-411">Debug</span></span>             | <span data-ttu-id="dfa44-412">Error</span><span class="sxs-lookup"><span data-stu-id="dfa44-412">Error</span></span>            |
| <span data-ttu-id="dfa44-413">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="dfa44-413">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="dfa44-414">Provider di configurazione XML</span><span class="sxs-lookup"><span data-stu-id="dfa44-414">XML Configuration Provider</span></span>

<span data-ttu-id="dfa44-415">Il <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> carica la configurazione da coppie chiave-valore di file XML in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-415">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="dfa44-416">Per attivare la configurazione di file XML, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-416">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="dfa44-417">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="dfa44-417">Overloads permit specifying:</span></span>

* <span data-ttu-id="dfa44-418">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="dfa44-418">Whether the file is optional.</span></span>
* <span data-ttu-id="dfa44-419">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="dfa44-419">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="dfa44-420">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="dfa44-420">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="dfa44-421">Il nodo radice del file di configurazione viene ignorato quando vengono create le coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-421">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="dfa44-422">Non specificare una definizione DTD (Document Type Definition) o uno spazio dei nomi nel file.</span><span class="sxs-lookup"><span data-stu-id="dfa44-422">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="dfa44-423">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="dfa44-423">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddXmlFile(
                    "config.xml", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="dfa44-424">Il percorso di base è impostato con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-424">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="dfa44-425">`SetBasePath` è incluso nel pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="dfa44-425">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="dfa44-426">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="dfa44-426">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="dfa44-427">Il percorso di base è impostato con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-427">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="dfa44-428">`SetBasePath` è incluso nel pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="dfa44-428">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="dfa44-429">I file di configurazione XML possono usare nomi di elementi distinti per le sezioni ripetute:</span><span class="sxs-lookup"><span data-stu-id="dfa44-429">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="dfa44-430">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="dfa44-430">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="dfa44-431">section0:key0</span><span class="sxs-lookup"><span data-stu-id="dfa44-431">section0:key0</span></span>
* <span data-ttu-id="dfa44-432">section0:key1</span><span class="sxs-lookup"><span data-stu-id="dfa44-432">section0:key1</span></span>
* <span data-ttu-id="dfa44-433">section1:key0</span><span class="sxs-lookup"><span data-stu-id="dfa44-433">section1:key0</span></span>
* <span data-ttu-id="dfa44-434">section1:key1</span><span class="sxs-lookup"><span data-stu-id="dfa44-434">section1:key1</span></span>

<span data-ttu-id="dfa44-435">La ripetizione di elementi che usano lo stesso nome di elemento funziona se si usa l'attributo `name` per distinguere gli elementi:</span><span class="sxs-lookup"><span data-stu-id="dfa44-435">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="dfa44-436">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="dfa44-436">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="dfa44-437">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="dfa44-437">section:section0:key:key0</span></span>
* <span data-ttu-id="dfa44-438">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="dfa44-438">section:section0:key:key1</span></span>
* <span data-ttu-id="dfa44-439">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="dfa44-439">section:section1:key:key0</span></span>
* <span data-ttu-id="dfa44-440">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="dfa44-440">section:section1:key:key1</span></span>

<span data-ttu-id="dfa44-441">È possibile usare attributi per fornire valori:</span><span class="sxs-lookup"><span data-stu-id="dfa44-441">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="dfa44-442">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="dfa44-442">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="dfa44-443">key:attribute</span><span class="sxs-lookup"><span data-stu-id="dfa44-443">key:attribute</span></span>
* <span data-ttu-id="dfa44-444">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="dfa44-444">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="dfa44-445">Provider di configurazione KeyPerFile</span><span class="sxs-lookup"><span data-stu-id="dfa44-445">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="dfa44-446">Il <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> usa i file di una directory come coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-446">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="dfa44-447">La chiave è il nome del file.</span><span class="sxs-lookup"><span data-stu-id="dfa44-447">The key is the file name.</span></span> <span data-ttu-id="dfa44-448">Il valore contiene il contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="dfa44-448">The value contains the file's contents.</span></span> <span data-ttu-id="dfa44-449">Il provider di configurazione KeyPerFile viene usato in scenari di hosting Docker.</span><span class="sxs-lookup"><span data-stu-id="dfa44-449">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="dfa44-450">Per attivare la configurazione chiave-per-file, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-450">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="dfa44-451">Il `directoryPath` per i file deve essere un percorso assoluto.</span><span class="sxs-lookup"><span data-stu-id="dfa44-451">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="dfa44-452">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="dfa44-452">Overloads permit specifying:</span></span>

* <span data-ttu-id="dfa44-453">Un delegato `Action<KeyPerFileConfigurationSource>` che configura l'origine.</span><span class="sxs-lookup"><span data-stu-id="dfa44-453">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="dfa44-454">Se la directory è facoltativa e il percorso della directory.</span><span class="sxs-lookup"><span data-stu-id="dfa44-454">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="dfa44-455">Il doppio carattere di sottolineatura (`__`) viene usato come delimitatore per le chiavi di configurazione nei nomi dei file.</span><span class="sxs-lookup"><span data-stu-id="dfa44-455">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="dfa44-456">Ad esempio, il nome di file `Logging__LogLevel__System` produce la chiave di configurazione `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-456">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="dfa44-457">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="dfa44-457">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                var path = Path.Combine(
                    Directory.GetCurrentDirectory(), "path/to/files");
                config.AddKeyPerFile(directoryPath: path, optional: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="dfa44-458">Il percorso di base è impostato con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-458">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="dfa44-459">`SetBasePath` è incluso nel pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="dfa44-459">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="dfa44-460">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="dfa44-460">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
var config = new ConfigurationBuilder()
    .AddKeyPerFile(directoryPath: path, optional: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="dfa44-461">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="dfa44-461">Memory Configuration Provider</span></span>

<span data-ttu-id="dfa44-462">Il <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> usa una raccolta in memoria come coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-462">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="dfa44-463">Per attivare la configurazione della raccolta in memoria, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-463">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="dfa44-464">Il provider di configurazione può essere inizializzato con un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-464">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="dfa44-465">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="dfa44-465">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _dict = 
        new Dictionary<string, string>
        {
            {"MemoryCollectionKey1", "value1"},
            {"MemoryCollectionKey2", "value2"}
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddInMemoryCollection(_dict);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="dfa44-466">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="dfa44-466">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

var config = new ConfigurationBuilder()
    .AddInMemoryCollection(dict)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

## <a name="getvalue"></a><span data-ttu-id="dfa44-467">GetValue</span><span class="sxs-lookup"><span data-stu-id="dfa44-467">GetValue</span></span>

<span data-ttu-id="dfa44-468">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) estrae un valore dalla configurazione con una chiave specificata e lo converte nel tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="dfa44-468">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="dfa44-469">Un overload consente di specificare un valore predefinito se non viene trovata la chiave.</span><span class="sxs-lookup"><span data-stu-id="dfa44-469">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="dfa44-470">L'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="dfa44-470">The following example:</span></span>

* <span data-ttu-id="dfa44-471">Estrae il valore di stringa dalla configurazione con la chiave `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-471">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="dfa44-472">Se `NumberKey` non viene trovato nelle chiavi di configurazione, viene usato il valore predefinito di `99`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-472">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="dfa44-473">Assegna al valore il tipo `int`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-473">Types the value as an `int`.</span></span>
* <span data-ttu-id="dfa44-474">Memorizza il valore nella proprietà `NumberConfig` per l'uso da parte della pagina.</span><span class="sxs-lookup"><span data-stu-id="dfa44-474">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="dfa44-475">GetSection, GetChildren ed Exists</span><span class="sxs-lookup"><span data-stu-id="dfa44-475">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="dfa44-476">Per gli esempi che seguono, prendere in considerazione il file JSON seguente.</span><span class="sxs-lookup"><span data-stu-id="dfa44-476">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="dfa44-477">Vengono trovate quattro chiavi in due sezioni, una delle quali include una coppia di sottosezioni:</span><span class="sxs-lookup"><span data-stu-id="dfa44-477">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="dfa44-478">Quando il file viene letto nella configurazione, vengono create le chiavi gerarchiche univoche seguenti per contenere i valori di configurazione:</span><span class="sxs-lookup"><span data-stu-id="dfa44-478">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="dfa44-479">section0:key0</span><span class="sxs-lookup"><span data-stu-id="dfa44-479">section0:key0</span></span>
* <span data-ttu-id="dfa44-480">section0:key1</span><span class="sxs-lookup"><span data-stu-id="dfa44-480">section0:key1</span></span>
* <span data-ttu-id="dfa44-481">section1:key0</span><span class="sxs-lookup"><span data-stu-id="dfa44-481">section1:key0</span></span>
* <span data-ttu-id="dfa44-482">section1:key1</span><span class="sxs-lookup"><span data-stu-id="dfa44-482">section1:key1</span></span>
* <span data-ttu-id="dfa44-483">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="dfa44-483">section2:subsection0:key0</span></span>
* <span data-ttu-id="dfa44-484">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="dfa44-484">section2:subsection0:key1</span></span>
* <span data-ttu-id="dfa44-485">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="dfa44-485">section2:subsection1:key0</span></span>
* <span data-ttu-id="dfa44-486">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="dfa44-486">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="dfa44-487">GetSection</span><span class="sxs-lookup"><span data-stu-id="dfa44-487">GetSection</span></span>

<span data-ttu-id="dfa44-488">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) estrae una sottosezione della configurazione con la chiave di sottosezione specificata.</span><span class="sxs-lookup"><span data-stu-id="dfa44-488">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span> <span data-ttu-id="dfa44-489">`GetSection` è incluso nel pacchetto [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="dfa44-489">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="dfa44-490">Per restituire una <xref:Microsoft.Extensions.Configuration.IConfigurationSection> che contiene solo le coppie chiave-valore in `section1`, chiamare `GetSection` e fornire il nome della sezione:</span><span class="sxs-lookup"><span data-stu-id="dfa44-490">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="dfa44-491">`configSection` non ha un valore, ma solo una chiave e un percorso.</span><span class="sxs-lookup"><span data-stu-id="dfa44-491">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="dfa44-492">In modo analogo, per ottenere i valori per le chiavi in `section2:subsection0`, chiamare `GetSection` e fornire il percorso della sezione:</span><span class="sxs-lookup"><span data-stu-id="dfa44-492">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="dfa44-493">`GetSection` non restituisce mai `null`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-493">`GetSection` never returns `null`.</span></span> <span data-ttu-id="dfa44-494">Se non viene trovata una sezione corrispondente, viene restituita una `IConfigurationSection` vuota.</span><span class="sxs-lookup"><span data-stu-id="dfa44-494">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="dfa44-495">Quando `GetSection` restituisce una sezione corrispondente, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> non viene compilata.</span><span class="sxs-lookup"><span data-stu-id="dfa44-495">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="dfa44-496">Quando la sezione esiste, vengono restituiti <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> e <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-496">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="dfa44-497">GetChildren</span><span class="sxs-lookup"><span data-stu-id="dfa44-497">GetChildren</span></span>

<span data-ttu-id="dfa44-498">Una chiamata a [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) per `section2` ottiene una `IEnumerable<IConfigurationSection>` che include:</span><span class="sxs-lookup"><span data-stu-id="dfa44-498">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="dfa44-499">Esistente</span><span class="sxs-lookup"><span data-stu-id="dfa44-499">Exists</span></span>

<span data-ttu-id="dfa44-500">Usare [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) per determinare se esiste una sezione di configurazione:</span><span class="sxs-lookup"><span data-stu-id="dfa44-500">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="dfa44-501">Considerati i dati di esempio, `sectionExists` è `false` perché non esiste una sezione `section2:subsection2` nei dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-501">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="dfa44-502">Eseguire l'associazione a una classe</span><span class="sxs-lookup"><span data-stu-id="dfa44-502">Bind to a class</span></span>

<span data-ttu-id="dfa44-503">La configurazione può essere associata alle classi che rappresentano gruppi di impostazioni correlate usando il *modello di opzioni*.</span><span class="sxs-lookup"><span data-stu-id="dfa44-503">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="dfa44-504">Per altre informazioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-504">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="dfa44-505">I valori di configurazione vengono restituiti come stringhe, ma la chiamata di <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> consente la costruzione di oggetti [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="dfa44-505">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="dfa44-506">`Bind` è incluso nel pacchetto [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="dfa44-506">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="dfa44-507">L'app di esempio contiene un modello `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="dfa44-507">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="dfa44-508">La sezione `starship` del file *starship.json* crea la configurazione quando l'app di esempio usa il provider di configurazione JSON per caricare la configurazione:</span><span class="sxs-lookup"><span data-stu-id="dfa44-508">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

<span data-ttu-id="dfa44-509">Vengono create le coppie chiave-valore della configurazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="dfa44-509">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="dfa44-510">Chiave</span><span class="sxs-lookup"><span data-stu-id="dfa44-510">Key</span></span>                   | <span data-ttu-id="dfa44-511">Value</span><span class="sxs-lookup"><span data-stu-id="dfa44-511">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="dfa44-512">starship:name</span><span class="sxs-lookup"><span data-stu-id="dfa44-512">starship:name</span></span>         | <span data-ttu-id="dfa44-513">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="dfa44-513">USS Enterprise</span></span>                                    |
| <span data-ttu-id="dfa44-514">starship:registry</span><span class="sxs-lookup"><span data-stu-id="dfa44-514">starship:registry</span></span>     | <span data-ttu-id="dfa44-515">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="dfa44-515">NCC-1701</span></span>                                          |
| <span data-ttu-id="dfa44-516">starship:class</span><span class="sxs-lookup"><span data-stu-id="dfa44-516">starship:class</span></span>        | <span data-ttu-id="dfa44-517">Constitution</span><span class="sxs-lookup"><span data-stu-id="dfa44-517">Constitution</span></span>                                      |
| <span data-ttu-id="dfa44-518">starship:length</span><span class="sxs-lookup"><span data-stu-id="dfa44-518">starship:length</span></span>       | <span data-ttu-id="dfa44-519">304.8</span><span class="sxs-lookup"><span data-stu-id="dfa44-519">304.8</span></span>                                             |
| <span data-ttu-id="dfa44-520">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="dfa44-520">starship:commissioned</span></span> | <span data-ttu-id="dfa44-521">False</span><span class="sxs-lookup"><span data-stu-id="dfa44-521">False</span></span>                                             |
| <span data-ttu-id="dfa44-522">trademark</span><span class="sxs-lookup"><span data-stu-id="dfa44-522">trademark</span></span>             | <span data-ttu-id="dfa44-523">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="dfa44-523">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="dfa44-524">L'app di esempio chiama `GetSection` con la chiave `starship`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-524">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="dfa44-525">Le coppie chiave-valore `starship` sono isolate.</span><span class="sxs-lookup"><span data-stu-id="dfa44-525">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="dfa44-526">Il metodo `Bind` viene chiamato sulla sottosezione passando un'istanza della classe `Starship`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-526">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="dfa44-527">Dopo aver associato i valori dell'istanza, l'istanza viene assegnata a una proprietà per il rendering:</span><span class="sxs-lookup"><span data-stu-id="dfa44-527">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

<span data-ttu-id="dfa44-528">`GetSection` è incluso nel pacchetto [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="dfa44-528">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="dfa44-529">Associazione a un oggetto grafico</span><span class="sxs-lookup"><span data-stu-id="dfa44-529">Bind to an object graph</span></span>

<span data-ttu-id="dfa44-530"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> è in grado di associare un intero oggetto grafico POCO.</span><span class="sxs-lookup"><span data-stu-id="dfa44-530"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="dfa44-531">`Bind` è incluso nel pacchetto [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="dfa44-531">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="dfa44-532">L'esempio contiene un modello `TvShow` il cui oggetto grafico include `Metadata` e le classi `Actors` (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="dfa44-532">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="dfa44-533">L'app di esempio ha un file *tvshow.xml* che contiene i dati di configurazione:</span><span class="sxs-lookup"><span data-stu-id="dfa44-533">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="dfa44-534">La configurazione viene associata all'intero oggetto grafico `TvShow` con il metodo `Bind`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-534">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="dfa44-535">L'istanza associata viene assegnata a una proprietà per il rendering:</span><span class="sxs-lookup"><span data-stu-id="dfa44-535">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="dfa44-536">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) associa e restituisce il tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="dfa44-536">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="dfa44-537">`Get<T>` può essere più comodo che usare `Bind`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-537">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="dfa44-538">Il codice seguente mostra come usare `Get<T>` con l'esempio precedente, che consente di assegnare l'istanza associata direttamente alla proprietà usata per il rendering:</span><span class="sxs-lookup"><span data-stu-id="dfa44-538">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

<span data-ttu-id="dfa44-539"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> è incluso nel pacchetto [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="dfa44-539"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="dfa44-540">`Get<T>` è disponibile in ASP.NET Core 1.1 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="dfa44-540">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="dfa44-541">`GetSection` è incluso nel pacchetto [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="dfa44-541">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="dfa44-542">Associare una matrice a una classe</span><span class="sxs-lookup"><span data-stu-id="dfa44-542">Bind an array to a class</span></span>

<span data-ttu-id="dfa44-543">*L'app di esempio dimostra i concetti spiegati in questa sezione.*</span><span class="sxs-lookup"><span data-stu-id="dfa44-543">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="dfa44-544">Il <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supporta l'associazione di matrici agli oggetti usando gli indici delle matrici nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-544">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="dfa44-545">Qualsiasi formato di matrice che espone un segmento chiave numerico (`:0:`, `:1:`, &hellip; `:{n}:`) supporta l'associazione di matrici a una matrice di classi POCO.</span><span class="sxs-lookup"><span data-stu-id="dfa44-545">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span> <span data-ttu-id="dfa44-546">"Bind" è incluso nel pacchetto [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="dfa44-546">\`Bind\`\` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

> [!NOTE]
> <span data-ttu-id="dfa44-547">L'associazione viene fornita per convenzione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-547">Binding is provided by convention.</span></span> <span data-ttu-id="dfa44-548">I provider di configurazione personalizzati non devono implementare l'associazione di matrici.</span><span class="sxs-lookup"><span data-stu-id="dfa44-548">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="dfa44-549">**Elaborazione di matrici in memoria**</span><span class="sxs-lookup"><span data-stu-id="dfa44-549">**In-memory array processing**</span></span>

<span data-ttu-id="dfa44-550">Prendere in considerazione le chiavi di configurazione e i valori indicati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="dfa44-550">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="dfa44-551">Chiave</span><span class="sxs-lookup"><span data-stu-id="dfa44-551">Key</span></span>             | <span data-ttu-id="dfa44-552">Value</span><span class="sxs-lookup"><span data-stu-id="dfa44-552">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="dfa44-553">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="dfa44-553">array:entries:0</span></span> | <span data-ttu-id="dfa44-554">value0</span><span class="sxs-lookup"><span data-stu-id="dfa44-554">value0</span></span> |
| <span data-ttu-id="dfa44-555">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="dfa44-555">array:entries:1</span></span> | <span data-ttu-id="dfa44-556">value1</span><span class="sxs-lookup"><span data-stu-id="dfa44-556">value1</span></span> |
| <span data-ttu-id="dfa44-557">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="dfa44-557">array:entries:2</span></span> | <span data-ttu-id="dfa44-558">value2</span><span class="sxs-lookup"><span data-stu-id="dfa44-558">value2</span></span> |
| <span data-ttu-id="dfa44-559">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="dfa44-559">array:entries:4</span></span> | <span data-ttu-id="dfa44-560">value4</span><span class="sxs-lookup"><span data-stu-id="dfa44-560">value4</span></span> |
| <span data-ttu-id="dfa44-561">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="dfa44-561">array:entries:5</span></span> | <span data-ttu-id="dfa44-562">value5</span><span class="sxs-lookup"><span data-stu-id="dfa44-562">value5</span></span> |

<span data-ttu-id="dfa44-563">Queste chiavi e i valori vengono caricati nell'app di esempio usando il provider di configurazione della memoria:</span><span class="sxs-lookup"><span data-stu-id="dfa44-563">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,23)]

<span data-ttu-id="dfa44-564">La matrice ignora un valore per l'indice &num;3.</span><span class="sxs-lookup"><span data-stu-id="dfa44-564">The array skips a value for index &num;3.</span></span> <span data-ttu-id="dfa44-565">Lo strumento di associazione di configurazione non è in grado di associare valori null o di creare voci null negli oggetti associati, situazione che diventerà chiara tra poco quando viene illustrato il risultato dell'associazione di questa matrice a un oggetto.</span><span class="sxs-lookup"><span data-stu-id="dfa44-565">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="dfa44-566">Nell'app di esempio, è disponibile una classe POCO per i dati di configurazione associati:</span><span class="sxs-lookup"><span data-stu-id="dfa44-566">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="dfa44-567">I dati di configurazione sono associati all'oggetto:</span><span class="sxs-lookup"><span data-stu-id="dfa44-567">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="dfa44-568">`GetSection` è incluso nel pacchetto [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="dfa44-568">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="dfa44-569">È possibile usare anche la sintassi [ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*), che produce codice più compatto:</span><span class="sxs-lookup"><span data-stu-id="dfa44-569">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="dfa44-570">L'oggetto associato, un'istanza di `ArrayExample`, riceve i dati di matrice dalla configurazione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-570">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="dfa44-571">Indice di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="dfa44-571">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="dfa44-572">Valore di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="dfa44-572">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="dfa44-573">0</span><span class="sxs-lookup"><span data-stu-id="dfa44-573">0</span></span>                            | <span data-ttu-id="dfa44-574">value0</span><span class="sxs-lookup"><span data-stu-id="dfa44-574">value0</span></span>                       |
| <span data-ttu-id="dfa44-575">1</span><span class="sxs-lookup"><span data-stu-id="dfa44-575">1</span></span>                            | <span data-ttu-id="dfa44-576">value1</span><span class="sxs-lookup"><span data-stu-id="dfa44-576">value1</span></span>                       |
| <span data-ttu-id="dfa44-577">2</span><span class="sxs-lookup"><span data-stu-id="dfa44-577">2</span></span>                            | <span data-ttu-id="dfa44-578">value2</span><span class="sxs-lookup"><span data-stu-id="dfa44-578">value2</span></span>                       |
| <span data-ttu-id="dfa44-579">3</span><span class="sxs-lookup"><span data-stu-id="dfa44-579">3</span></span>                            | <span data-ttu-id="dfa44-580">value4</span><span class="sxs-lookup"><span data-stu-id="dfa44-580">value4</span></span>                       |
| <span data-ttu-id="dfa44-581">4</span><span class="sxs-lookup"><span data-stu-id="dfa44-581">4</span></span>                            | <span data-ttu-id="dfa44-582">value5</span><span class="sxs-lookup"><span data-stu-id="dfa44-582">value5</span></span>                       |

<span data-ttu-id="dfa44-583">L'indice &num;3 nell'oggetto associato contiene i dati di configurazione per la chiave di configurazione `array:4` e il relativo valore `value4`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-583">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="dfa44-584">Quando i dati di configurazione contenenti una matrice vengono associati, gli indici di matrice nelle chiavi di configurazione vengono usati semplicemente per scorrere i dati di configurazione quando si crea l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="dfa44-584">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="dfa44-585">Un valore null non può essere mantenuto nei dati di configurazione e una voce con valore null non viene creata in un oggetto associato quando una matrice nelle chiavi di configurazione ignora uno o più indici.</span><span class="sxs-lookup"><span data-stu-id="dfa44-585">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="dfa44-586">L'elemento di configurazione mancante per l'indice &num;3 può essere fornito prima dell'associazione all'istanza `ArrayExample` da qualsiasi provider di configurazione che produce la coppia chiave-valore corretta nella configurazione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-586">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="dfa44-587">Se il codice di esempio include un provider di configurazione JSON aggiuntivo con la coppia chiave-valore mancante, `ArrayExample.Entries` corrisponde alla matrice di configurazione completa:</span><span class="sxs-lookup"><span data-stu-id="dfa44-587">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="dfa44-588">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="dfa44-588">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="dfa44-589">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="dfa44-589">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="dfa44-590">La coppia chiave-valore mostrata nella tabella viene caricata nella configurazione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-590">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="dfa44-591">Chiave</span><span class="sxs-lookup"><span data-stu-id="dfa44-591">Key</span></span>             | <span data-ttu-id="dfa44-592">Value</span><span class="sxs-lookup"><span data-stu-id="dfa44-592">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="dfa44-593">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="dfa44-593">array:entries:3</span></span> | <span data-ttu-id="dfa44-594">value3</span><span class="sxs-lookup"><span data-stu-id="dfa44-594">value3</span></span> |

<span data-ttu-id="dfa44-595">Se l'istanza della classe `ArrayExample` viene associata dopo che il provider di configurazione JSON include la voce per l'indice &num;3, la matrice `ArrayExample.Entries` include il valore.</span><span class="sxs-lookup"><span data-stu-id="dfa44-595">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="dfa44-596">Indice di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="dfa44-596">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="dfa44-597">Valore di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="dfa44-597">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="dfa44-598">0</span><span class="sxs-lookup"><span data-stu-id="dfa44-598">0</span></span>                            | <span data-ttu-id="dfa44-599">value0</span><span class="sxs-lookup"><span data-stu-id="dfa44-599">value0</span></span>                       |
| <span data-ttu-id="dfa44-600">1</span><span class="sxs-lookup"><span data-stu-id="dfa44-600">1</span></span>                            | <span data-ttu-id="dfa44-601">value1</span><span class="sxs-lookup"><span data-stu-id="dfa44-601">value1</span></span>                       |
| <span data-ttu-id="dfa44-602">2</span><span class="sxs-lookup"><span data-stu-id="dfa44-602">2</span></span>                            | <span data-ttu-id="dfa44-603">value2</span><span class="sxs-lookup"><span data-stu-id="dfa44-603">value2</span></span>                       |
| <span data-ttu-id="dfa44-604">3</span><span class="sxs-lookup"><span data-stu-id="dfa44-604">3</span></span>                            | <span data-ttu-id="dfa44-605">value3</span><span class="sxs-lookup"><span data-stu-id="dfa44-605">value3</span></span>                       |
| <span data-ttu-id="dfa44-606">4</span><span class="sxs-lookup"><span data-stu-id="dfa44-606">4</span></span>                            | <span data-ttu-id="dfa44-607">value4</span><span class="sxs-lookup"><span data-stu-id="dfa44-607">value4</span></span>                       |
| <span data-ttu-id="dfa44-608">5</span><span class="sxs-lookup"><span data-stu-id="dfa44-608">5</span></span>                            | <span data-ttu-id="dfa44-609">value5</span><span class="sxs-lookup"><span data-stu-id="dfa44-609">value5</span></span>                       |

<span data-ttu-id="dfa44-610">**Elaborazione di matrice JSON**</span><span class="sxs-lookup"><span data-stu-id="dfa44-610">**JSON array processing**</span></span>

<span data-ttu-id="dfa44-611">Se un file JSON contiene una matrice, vengono create chiavi di configurazione per gli elementi della matrice con un indice di sezione in base zero.</span><span class="sxs-lookup"><span data-stu-id="dfa44-611">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="dfa44-612">Nel file di configurazione seguente, `subsection` è una matrice:</span><span class="sxs-lookup"><span data-stu-id="dfa44-612">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="dfa44-613">Il provider di configurazione JSON legge i dati di configurazione nelle coppie chiave-valore seguenti:</span><span class="sxs-lookup"><span data-stu-id="dfa44-613">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="dfa44-614">Chiave</span><span class="sxs-lookup"><span data-stu-id="dfa44-614">Key</span></span>                     | <span data-ttu-id="dfa44-615">Value</span><span class="sxs-lookup"><span data-stu-id="dfa44-615">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="dfa44-616">json_array:key</span><span class="sxs-lookup"><span data-stu-id="dfa44-616">json_array:key</span></span>          | <span data-ttu-id="dfa44-617">valueA</span><span class="sxs-lookup"><span data-stu-id="dfa44-617">valueA</span></span> |
| <span data-ttu-id="dfa44-618">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="dfa44-618">json_array:subsection:0</span></span> | <span data-ttu-id="dfa44-619">valueB</span><span class="sxs-lookup"><span data-stu-id="dfa44-619">valueB</span></span> |
| <span data-ttu-id="dfa44-620">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="dfa44-620">json_array:subsection:1</span></span> | <span data-ttu-id="dfa44-621">valueC</span><span class="sxs-lookup"><span data-stu-id="dfa44-621">valueC</span></span> |
| <span data-ttu-id="dfa44-622">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="dfa44-622">json_array:subsection:2</span></span> | <span data-ttu-id="dfa44-623">valueD</span><span class="sxs-lookup"><span data-stu-id="dfa44-623">valueD</span></span> |

<span data-ttu-id="dfa44-624">Nell'app di esempio, la classe POCO seguente è disponibile per associare le coppie chiave-valore della configurazione:</span><span class="sxs-lookup"><span data-stu-id="dfa44-624">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="dfa44-625">Dopo l'associazione, `JsonArrayExample.Key` contiene il valore `valueA`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-625">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="dfa44-626">I valori di sottosezione vengono archiviati nella proprietà matrice POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-626">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="dfa44-627">Indice di `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="dfa44-627">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="dfa44-628">Valore di `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="dfa44-628">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="dfa44-629">0</span><span class="sxs-lookup"><span data-stu-id="dfa44-629">0</span></span>                                   | <span data-ttu-id="dfa44-630">valueB</span><span class="sxs-lookup"><span data-stu-id="dfa44-630">valueB</span></span>                              |
| <span data-ttu-id="dfa44-631">1</span><span class="sxs-lookup"><span data-stu-id="dfa44-631">1</span></span>                                   | <span data-ttu-id="dfa44-632">valueC</span><span class="sxs-lookup"><span data-stu-id="dfa44-632">valueC</span></span>                              |
| <span data-ttu-id="dfa44-633">2</span><span class="sxs-lookup"><span data-stu-id="dfa44-633">2</span></span>                                   | <span data-ttu-id="dfa44-634">valueD</span><span class="sxs-lookup"><span data-stu-id="dfa44-634">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="dfa44-635">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="dfa44-635">Custom configuration provider</span></span>

<span data-ttu-id="dfa44-636">L'app di esempio dimostra come creare un provider di configurazione di base che legge le coppie chiave-valore di configurazione da un database usando [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="dfa44-636">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="dfa44-637">Il provider ha le caratteristiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="dfa44-637">The provider has the following characteristics:</span></span>

* <span data-ttu-id="dfa44-638">Il database in memoria di Entity Framework viene usato a scopo dimostrativo.</span><span class="sxs-lookup"><span data-stu-id="dfa44-638">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="dfa44-639">Per usare un database che richiede una stringa di connessione, implementare un `ConfigurationBuilder` secondario per fornire la stringa di connessione da un altro provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-639">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="dfa44-640">Il provider legge una tabella di database in una configurazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="dfa44-640">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="dfa44-641">Il provider non esegue una query sul database per ogni chiave.</span><span class="sxs-lookup"><span data-stu-id="dfa44-641">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="dfa44-642">Il ricaricamento in caso di modifica non è implementato, quindi l'aggiornamento del database dopo l'avvio dell'app non ha alcun effetto sulla configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="dfa44-642">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="dfa44-643">Definire un'entità `EFConfigurationValue` per l'archiviazione dei valori di configurazione nel database.</span><span class="sxs-lookup"><span data-stu-id="dfa44-643">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="dfa44-644">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="dfa44-644">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="dfa44-645">Aggiungere `EFConfigurationContext` per archiviare i valori configurati e accedervi.</span><span class="sxs-lookup"><span data-stu-id="dfa44-645">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="dfa44-646">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="dfa44-646">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="dfa44-647">Creare una classe che implementi <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-647">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="dfa44-648">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="dfa44-648">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="dfa44-649">Creare il provider di configurazione personalizzato ereditando da <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-649">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="dfa44-650">Il provider di configurazione inizializza il database quando è vuoto.</span><span class="sxs-lookup"><span data-stu-id="dfa44-650">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="dfa44-651">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="dfa44-651">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="dfa44-652">Un metodo di estensione `AddEFConfiguration` consente di aggiungere l'origine di configurazione a un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-652">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="dfa44-653">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="dfa44-653">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="dfa44-654">L'esempio di codice seguente mostra come usare il `EFConfigurationProvider` personalizzato in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="dfa44-654">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=30-31)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="dfa44-655">Accedere alla configurazione durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="dfa44-655">Access configuration during startup</span></span>

<span data-ttu-id="dfa44-656">Inserire `IConfiguration` nel costruttore `Startup` per accedere ai valori di configurazione in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="dfa44-656">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="dfa44-657">Per accedere alla configurazione in `Startup.Configure`, inserire `IConfiguration` direttamente nel metodo o usare l'istanza dal costruttore:</span><span class="sxs-lookup"><span data-stu-id="dfa44-657">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="dfa44-658">Per un esempio di accesso alla configurazione usando metodi di servizio di avvio, vedere [Avvio dell'applicazione: Metodi pratici](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="dfa44-658">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="dfa44-659">Accedere alla configurazione in una pagina Razor Pages o in una visualizzazione MVC</span><span class="sxs-lookup"><span data-stu-id="dfa44-659">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="dfa44-660">Per accedere alle impostazioni di configurazione in una pagina Razor Pages o in una visualizzazione MVC, aggiungere un [direttiva using](xref:mvc/views/razor#using) ([Informazioni di riferimento su C#: direttiva using](/dotnet/csharp/language-reference/keywords/using-directive)) per lo [spazio dei nomi Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) e inserire <xref:Microsoft.Extensions.Configuration.IConfiguration> nella pagina o nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="dfa44-660">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="dfa44-661">In una pagina Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="dfa44-661">In a Razor Pages page:</span></span>

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

<span data-ttu-id="dfa44-662">In una visualizzazione MVC:</span><span class="sxs-lookup"><span data-stu-id="dfa44-662">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="dfa44-663">Aggiungere la configurazione da un assembly esterno</span><span class="sxs-lookup"><span data-stu-id="dfa44-663">Add configuration from an external assembly</span></span>

<span data-ttu-id="dfa44-664">Un'implementazione <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> consente l'aggiunta di miglioramenti a un'app all'avvio, da un assembly esterno alla classe `Startup` dell'app.</span><span class="sxs-lookup"><span data-stu-id="dfa44-664">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="dfa44-665">Per altre informazioni, vedere <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="dfa44-665">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dfa44-666">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="dfa44-666">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* <span data-ttu-id="dfa44-667">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/) (Configurazione Microsoft in dettaglio)</span><span class="sxs-lookup"><span data-stu-id="dfa44-667">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)</span></span>
