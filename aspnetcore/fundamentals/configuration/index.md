---
title: Configurazione in ASP.NET Core
author: guardrex
description: Informazioni su come usare l'API di configurazione per configurare un'app ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/11/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 63a876c09f952537d790f2a5df4b8672df49d015
ms.sourcegitcommit: 3376f224b47a89acf329b2d2f9260046a372f924
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/10/2019
ms.locfileid: "65517026"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="1ebbd-103">Configurazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ebbd-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="1ebbd-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1ebbd-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1ebbd-105">La configurazione delle app in ASP.NET Core si basa su coppie chiave-valore stabilite dai *provider di configurazione*.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="1ebbd-106">I provider di configurazione leggono i dati di configurazione in coppie chiave-valore da un'ampia gamma di origini di configurazione:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="1ebbd-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="1ebbd-107">Azure Key Vault</span></span>
* <span data-ttu-id="1ebbd-108">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="1ebbd-108">Command-line arguments</span></span>
* <span data-ttu-id="1ebbd-109">Provider personalizzati (installati o creati)</span><span class="sxs-lookup"><span data-stu-id="1ebbd-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="1ebbd-110">File della directory</span><span class="sxs-lookup"><span data-stu-id="1ebbd-110">Directory files</span></span>
* <span data-ttu-id="1ebbd-111">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="1ebbd-111">Environment variables</span></span>
* <span data-ttu-id="1ebbd-112">Oggetti .NET in memoria</span><span class="sxs-lookup"><span data-stu-id="1ebbd-112">In-memory .NET objects</span></span>
* <span data-ttu-id="1ebbd-113">File di impostazioni</span><span class="sxs-lookup"><span data-stu-id="1ebbd-113">Settings files</span></span>

<span data-ttu-id="1ebbd-114">Il *modello di opzioni* è un'estensione dei concetti di configurazione descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-114">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="1ebbd-115">Le opzioni usano le classi per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-115">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="1ebbd-116">Per altre informazioni sull'uso del modello di opzioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-116">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="1ebbd-117">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1ebbd-117">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1ebbd-118">Questi tre pacchetti sono inclusi nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-118">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="host-vs-app-configuration"></a><span data-ttu-id="1ebbd-119">Host e configurazione delle app</span><span class="sxs-lookup"><span data-stu-id="1ebbd-119">Host vs. app configuration</span></span>

<span data-ttu-id="1ebbd-120">Prima che l'app venga configurata e avviata, viene configurato e avviato un *host*.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-120">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="1ebbd-121">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-121">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="1ebbd-122">Sia l'app che l'host vengono configurati tramite i provider di configurazione descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-122">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="1ebbd-123">Le coppie chiave-valore di configurazione dell'host diventano parte della configurazione globale dell'app.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-123">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="1ebbd-124">Per altre informazioni su come vengono usati i provider di configurazione per la creazione dell'host e sugli effetti delle origini di configurazione sulla configurazione dell'host, vedere [L'host](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-124">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see [The host](xref:fundamentals/index#host).</span></span>

## <a name="default-configuration"></a><span data-ttu-id="1ebbd-125">Configurazione predefinita</span><span class="sxs-lookup"><span data-stu-id="1ebbd-125">Default configuration</span></span>

<span data-ttu-id="1ebbd-126">Le app basate sui modelli [dotnet new](/dotnet/core/tools/dotnet-new) di ASP.NET Core chiamano <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> durante la compilazione di un host.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-126">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="1ebbd-127"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> fornisce la configurazione predefinita per l'app nell'ordine seguente:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-127"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> provides default configuration for the app in the following order:</span></span>

* <span data-ttu-id="1ebbd-128">La configurazione dell'host viene specificata da:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-128">Host configuration is provided from:</span></span>
  * <span data-ttu-id="1ebbd-129">Variabili di ambiente con prefisso `ASPNETCORE_` (ad esempio `ASPNETCORE_ENVIRONMENT`) che usano il [provider di configurazione delle variabili di ambiente](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-129">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="1ebbd-130">Il prefisso (`ASPNETCORE_`) viene rimosso al caricamento delle coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-130">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="1ebbd-131">Argomenti della riga di comando che usano il [provider di configurazione della riga di comando](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-131">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="1ebbd-132">La configurazione dell'app viene fornita da:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-132">App configuration is provided from:</span></span>
  * <span data-ttu-id="1ebbd-133">*appsettings.json* mediante il [provider di configurazione dei file](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-133">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="1ebbd-134">*appsettings.{Ambiente}.json* mediante il [provider di configurazione dei file](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-134">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="1ebbd-135">[Strumento di gestione dei segreti](xref:security/app-secrets) quando l'app viene eseguita nell'ambiente `Development` usando l'assembly di ingresso.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="1ebbd-136">Variabili di ambiente che usano il [provider di configurazione delle variabili di ambiente](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-136">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="1ebbd-137">Se viene usato un prefisso personalizzato (ad esempio, `PREFIX_` con `.AddEnvironmentVariables(prefix: "PREFIX_")`), il prefisso viene rimosso al caricamento delle coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-137">If a custom prefix is used (for example, `PREFIX_` with `.AddEnvironmentVariables(prefix: "PREFIX_")`), the prefix is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="1ebbd-138">Argomenti della riga di comando che usano il [provider di configurazione della riga di comando](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-138">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="1ebbd-139">I provider di configurazione sono descritti più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-139">The configuration providers are explained later in this topic.</span></span> <span data-ttu-id="1ebbd-140">Per altre informazioni sull'host e su <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, vedere <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-140">For more information on the host and <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="security"></a><span data-ttu-id="1ebbd-141">Sicurezza</span><span class="sxs-lookup"><span data-stu-id="1ebbd-141">Security</span></span>

<span data-ttu-id="1ebbd-142">Adottare le procedure consigliate seguenti:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-142">Adopt the following best practices:</span></span>

* <span data-ttu-id="1ebbd-143">Non archiviare mai la password o altri dati sensibili nel codice del provider di configurazione o in file di configurazione di testo normale.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-143">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="1ebbd-144">Non usare i segreti di produzione in ambienti di sviluppo o di test.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-144">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="1ebbd-145">Specificare i segreti all'esterno del progetto in modo che non possano essere inavvertitamente inviati a un repository del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-145">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="1ebbd-146">Altre informazioni su [come usare più ambienti](xref:fundamentals/environments) e la gestione dell'[archiviazione sicura dei segreti delle app durante lo sviluppo con Secret Manager](xref:security/app-secrets) (include consigli sull'uso delle variabili di ambiente per l'archiviazione di dati sensibili).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-146">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="1ebbd-147">Secret Manager usa il provider di configurazione dei file per archiviare i segreti utente in un file JSON nel sistema locale.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-147">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="1ebbd-148">Il provider di configurazione dei file è descritto più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-148">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="1ebbd-149">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) è una delle opzioni per l'archiviazione sicura dei segreti delle app.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-149">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="1ebbd-150">Per ulteriori informazioni, vedere <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-150">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="1ebbd-151">Dati di configurazione gerarchici</span><span class="sxs-lookup"><span data-stu-id="1ebbd-151">Hierarchical configuration data</span></span>

<span data-ttu-id="1ebbd-152">L'API di configurazione è in grado di mantenere i dati di configurazione gerarchici rendendoli flat grazie all'uso di un delimitatore nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-152">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="1ebbd-153">Nel file JSON seguente, esistono quattro chiavi in una gerarchia strutturata di due sezioni:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-153">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="1ebbd-154">Quando il file viene letto nella configurazione, vengono create chiavi univoche per mantenere la struttura di dati gerarchici originale dell'origine di configurazione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-154">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="1ebbd-155">Le sezioni e le chiavi vengono rese flat usando due punti (`:`) per mantenere la struttura originale:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-155">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="1ebbd-156">section0:key0</span><span class="sxs-lookup"><span data-stu-id="1ebbd-156">section0:key0</span></span>
* <span data-ttu-id="1ebbd-157">section0:key1</span><span class="sxs-lookup"><span data-stu-id="1ebbd-157">section0:key1</span></span>
* <span data-ttu-id="1ebbd-158">section1:key0</span><span class="sxs-lookup"><span data-stu-id="1ebbd-158">section1:key0</span></span>
* <span data-ttu-id="1ebbd-159">section1:key1</span><span class="sxs-lookup"><span data-stu-id="1ebbd-159">section1:key1</span></span>

<span data-ttu-id="1ebbd-160">I metodi <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> e <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> sono disponibili per isolare le sezioni e gli elementi figlio di una sezione nei dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-160"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="1ebbd-161">Questi metodi sono descritti più avanti in [GetSection, GetChildren ed Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-161">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span> <span data-ttu-id="1ebbd-162">`GetSection` è incluso nel pacchetto [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-162">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="conventions"></a><span data-ttu-id="1ebbd-163">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="1ebbd-163">Conventions</span></span>

<span data-ttu-id="1ebbd-164">All'avvio dell'app, le origini di configurazione vengono lette nell'ordine con cui sono specificati i rispettivi provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-164">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="1ebbd-165">I provider di configurazione che implementano il rilevamento delle modifiche sono in grado di ricaricare la configurazione quando viene modificata un'impostazione sottostante.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-165">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="1ebbd-166">Ad esempio, il provider di configurazione dei file (descritto più avanti in questo argomento) e il [provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration) implementano il rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-166">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="1ebbd-167"><xref:Microsoft.Extensions.Configuration.IConfiguration> è disponibile nel contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) dell'app.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-167"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="1ebbd-168"><xref:Microsoft.Extensions.Configuration.IConfiguration> può essere inserito in <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> di Razor Pages per ottenere la configurazione per la classe:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-168"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

```csharp
// using Microsoft.Extensions.Configuration;

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

<span data-ttu-id="1ebbd-169">I provider di configurazione non possono usare l'inserimento delle dipendenze, perché non è disponibile quando vengono configurati dall'host.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-169">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="1ebbd-170">Le chiavi di configurazione adottano le convenzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-170">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="1ebbd-171">Per le chiavi non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-171">Keys are case-insensitive.</span></span> <span data-ttu-id="1ebbd-172">Ad esempio, `ConnectionString` e `connectionstring` vengono considerate chiavi equivalenti.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-172">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="1ebbd-173">Se un valore per la stessa chiave viene impostato dallo stesso provider di configurazione o da provider diversi, viene usato l'ultimo valore impostato per la chiave.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-173">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="1ebbd-174">Chiavi gerarchiche</span><span class="sxs-lookup"><span data-stu-id="1ebbd-174">Hierarchical keys</span></span>
  * <span data-ttu-id="1ebbd-175">Nell'ambito dell'API di configurazione, il separatore due punti (`:`) funziona in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-175">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="1ebbd-176">Nelle variabili di ambiente, un separatore due punti potrebbe non funzionare in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-176">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="1ebbd-177">Il doppio carattere di sottolineatura (`__`) è supportato da tutte le piattaforme e viene convertito in due punti.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-177">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="1ebbd-178">In Azure Key Vault, le chiavi gerarchiche usano `--` (due trattini) come separatore.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-178">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="1ebbd-179">È necessario fornire il codice per sostituire i trattini con due punti quando i segreti vengono caricati nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-179">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="1ebbd-180">Il <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supporta l'associazione di matrici agli oggetti usando gli indici delle matrici nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-180">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="1ebbd-181">L'associazione di matrici è descritta nella sezione [Associare una matrice a una classe](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-181">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="1ebbd-182">I valori di configurazione adottano le convenzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-182">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="1ebbd-183">I valori sono stringhe.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-183">Values are strings.</span></span>
* <span data-ttu-id="1ebbd-184">I valori null non possono essere archiviati nella configurazione o associati a oggetti.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-184">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="1ebbd-185">Provider</span><span class="sxs-lookup"><span data-stu-id="1ebbd-185">Providers</span></span>

<span data-ttu-id="1ebbd-186">La tabella seguente mostra i provider di configurazione disponibili per le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-186">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="1ebbd-187">Provider</span><span class="sxs-lookup"><span data-stu-id="1ebbd-187">Provider</span></span> | <span data-ttu-id="1ebbd-188">Fornisce la configurazione da&hellip;</span><span class="sxs-lookup"><span data-stu-id="1ebbd-188">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="1ebbd-189">[Provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="1ebbd-189">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="1ebbd-190">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="1ebbd-190">Azure Key Vault</span></span> |
| [<span data-ttu-id="1ebbd-191">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="1ebbd-191">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="1ebbd-192">Parametri della riga di comando</span><span class="sxs-lookup"><span data-stu-id="1ebbd-192">Command-line parameters</span></span> |
| [<span data-ttu-id="1ebbd-193">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="1ebbd-193">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="1ebbd-194">Origine personalizzata</span><span class="sxs-lookup"><span data-stu-id="1ebbd-194">Custom source</span></span> |
| [<span data-ttu-id="1ebbd-195">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="1ebbd-195">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="1ebbd-196">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="1ebbd-196">Environment variables</span></span> |
| [<span data-ttu-id="1ebbd-197">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="1ebbd-197">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="1ebbd-198">File (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="1ebbd-198">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="1ebbd-199">Provider di configurazione chiave-per-file</span><span class="sxs-lookup"><span data-stu-id="1ebbd-199">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="1ebbd-200">File della directory</span><span class="sxs-lookup"><span data-stu-id="1ebbd-200">Directory files</span></span> |
| [<span data-ttu-id="1ebbd-201">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="1ebbd-201">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="1ebbd-202">Raccolte in memoria</span><span class="sxs-lookup"><span data-stu-id="1ebbd-202">In-memory collections</span></span> |
| <span data-ttu-id="1ebbd-203">[Segreti utente (Secret Manager)](xref:security/app-secrets) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="1ebbd-203">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="1ebbd-204">File nella directory dei profili utente</span><span class="sxs-lookup"><span data-stu-id="1ebbd-204">File in the user profile directory</span></span> |

<span data-ttu-id="1ebbd-205">Le origini di configurazione vengono lette nell'ordine in cui vengono specificati i rispetti provider di configurazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-205">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="1ebbd-206">I provider di configurazione descritti in questo argomento vengono presentati in ordine alfabetico e non nell'ordine in cui potrebbe disporli il codice.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-206">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="1ebbd-207">Ordinare i provider di configurazione nel codice in base alle specifiche priorità per le origini di configurazione sottostanti.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-207">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="1ebbd-208">Una sequenza tipica di provider di configurazione è:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-208">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="1ebbd-209">File (*appsettings.json*, *appsettings.{Environment}.json*, dove `{Environment}` è l'ambiente di hosting corrente dell'app)</span><span class="sxs-lookup"><span data-stu-id="1ebbd-209">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="1ebbd-210">Insieme di credenziali delle chiavi di Azure</span><span class="sxs-lookup"><span data-stu-id="1ebbd-210">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="1ebbd-211">[Segreti utente (Secret Manager)](xref:security/app-secrets) (sono nell'ambiente Development)</span><span class="sxs-lookup"><span data-stu-id="1ebbd-211">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="1ebbd-212">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="1ebbd-212">Environment variables</span></span>
1. <span data-ttu-id="1ebbd-213">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="1ebbd-213">Command-line arguments</span></span>

<span data-ttu-id="1ebbd-214">È pratica comune posizionare il provider di configurazione della riga di comando per ultimo in una serie di provider per consentire agli argomenti della riga di comando di sostituire la configurazione impostata da altri provider.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-214">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="1ebbd-215">Questa sequenza di provider viene applicata quando si inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-215">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="1ebbd-216">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-216">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

## <a name="configureappconfiguration"></a><span data-ttu-id="1ebbd-217">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="1ebbd-217">ConfigureAppConfiguration</span></span>

<span data-ttu-id="1ebbd-218">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare i provider di configurazione dell'app, oltre a quelli aggiunti automaticamente da <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-218">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

<span data-ttu-id="1ebbd-219">La configurazione fornita all'app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> è disponibile durante l'avvio dell'app, incluso `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-219">Configuration supplied to the app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1ebbd-220">Per altre informazioni, vedere la sezione [Accedere alla configurazione durante l'avvio](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-220">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="1ebbd-221">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="1ebbd-221">Command-line Configuration Provider</span></span>

<span data-ttu-id="1ebbd-222"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> carica la configurazione da coppie chiave-valore di argomenti della riga di comando in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-222">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="1ebbd-223">Per attivare la configurazione della riga di comando, metodo di estensione <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> viene chiamato su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-223">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1ebbd-224">`AddCommandLine` viene chiamato automaticamente quando si Inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-224">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="1ebbd-225">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-225">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="1ebbd-226">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-226">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="1ebbd-227">Una configurazione facoltativa da *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-227">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="1ebbd-228">[Segreti utente (Secret Manager)](xref:security/app-secrets) (nell'ambiente Development).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-228">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="1ebbd-229">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-229">Environment variables.</span></span>

<span data-ttu-id="1ebbd-230">`CreateDefaultBuilder` aggiunge il provider di configurazione della riga di comando per ultimo.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-230">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="1ebbd-231">Gli argomenti della riga di comando passati in fase di esecuzione sostituiscono la configurazione impostata dagli altri provider.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-231">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="1ebbd-232">`CreateDefaultBuilder` opera durante la creazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-232">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="1ebbd-233">Pertanto, la configurazione della riga di comando attivata da `CreateDefaultBuilder` può influire sul modo in cui l'host viene configurato.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-233">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="1ebbd-234">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-234">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="1ebbd-235">`AddCommandLine` è già stato chiamato da `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-235">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="1ebbd-236">Se è necessario specificare la configurazione dell'app ed è ancora possibile eseguire l'override della configurazione con argomenti della riga di comando, chiamare i provider aggiuntivi dell'app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> e chiamare infine `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-236">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="1ebbd-237">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-237">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="1ebbd-238">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="1ebbd-238">**Example**</span></span>

<span data-ttu-id="1ebbd-239">L'app di esempio consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include una chiamata a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-239">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="1ebbd-240">Aprire un prompt dei comandi nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-240">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="1ebbd-241">Fornire un argomento della riga di comando per il comando `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-241">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="1ebbd-242">Dopo che l'app è in esecuzione, aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-242">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="1ebbd-243">Notare che l'output contiene la coppia chiave-valore per l'argomento della riga di comando di configurazione fornito per `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-243">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="1ebbd-244">Argomenti</span><span class="sxs-lookup"><span data-stu-id="1ebbd-244">Arguments</span></span>

<span data-ttu-id="1ebbd-245">Il valore deve seguire un segno di uguale (`=`) o la chiave deve avere un prefisso (`--` o `/`) quando il valore segue uno spazio.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-245">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="1ebbd-246">Il valore può essere null se viene usato un segno di uguale (ad esempio, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-246">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="1ebbd-247">Prefisso chiave</span><span class="sxs-lookup"><span data-stu-id="1ebbd-247">Key prefix</span></span>               | <span data-ttu-id="1ebbd-248">Esempio</span><span class="sxs-lookup"><span data-stu-id="1ebbd-248">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="1ebbd-249">Nessun prefisso</span><span class="sxs-lookup"><span data-stu-id="1ebbd-249">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="1ebbd-250">Due trattini (`--`)</span><span class="sxs-lookup"><span data-stu-id="1ebbd-250">Two dashes (`--`)</span></span>        | <span data-ttu-id="1ebbd-251">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="1ebbd-251">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="1ebbd-252">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="1ebbd-252">Forward slash (`/`)</span></span>      | <span data-ttu-id="1ebbd-253">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="1ebbd-253">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="1ebbd-254">Nello stesso comando, non mischiare coppie chiave-valore di argomenti della riga di comando che usano un segno di uguale con coppie chiave-valore che usano uno spazio.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-254">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="1ebbd-255">Comandi di esempio:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-255">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="1ebbd-256">Mapping di sostituzione</span><span class="sxs-lookup"><span data-stu-id="1ebbd-256">Switch mappings</span></span>

<span data-ttu-id="1ebbd-257">I mapping di sostituzione consentono di usare la logica di sostituzione del nome della chiave.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-257">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="1ebbd-258">Quando si crea manualmente la configurazione con un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, è possibile specificare un dizionario di mapping di sostituzione per il metodo <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-258">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="1ebbd-259">Quando viene utilizzato il dizionario dei mapping di sostituzione, nel dizionario viene controllata la presenza di una chiave corrispondente alla chiave fornita da un argomento della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-259">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="1ebbd-260">Se la chiave della riga di comando viene trovata nel dizionario, il valore del dizionario (la sostituzione della chiave) viene passato nuovamente per impostare la coppia chiave-valore nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-260">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="1ebbd-261">Un mapping di sostituzione è necessario per le chiavi della riga di comando con un trattino singolo (`-`) come prefisso.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-261">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="1ebbd-262">Regole principali del dizionario dei mapping di sostituzione:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-262">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="1ebbd-263">Le sostituzioni devono iniziare con un trattino (`-`) o un trattino doppio (`--`).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-263">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="1ebbd-264">Il dizionario dei mapping di sostituzione non deve contenere chiavi duplicate.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-264">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="1ebbd-265">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-265">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="1ebbd-266">Come illustrato nell'esempio precedente, la chiamata a `CreateDefaultBuilder` non deve passare argomenti quando vengono usati i mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-266">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="1ebbd-267">La chiamata `AddCommandLine` del metodo `CreateDefaultBuilder` non include sostituzioni mappate e non è possibile passare il dizionario dei mapping di sostituzione a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-267">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="1ebbd-268">Se gli argomenti includono una sostituzione mappata e vengono passati a `CreateDefaultBuilder`, l'inizializzazione del rispettivo provider `AddCommandLine` non riesce con <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-268">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="1ebbd-269">La soluzione consiste nel non passare gli argomenti a `CreateDefaultBuilder` consentendo invece al metodo `AddCommandLine` del metodo `ConfigurationBuilder` di elaborare entrambi gli argomenti e il dizionario dei mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-269">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="1ebbd-270">Il dizionario dei mapping di sostituzione creato contiene i dati visualizzati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-270">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="1ebbd-271">Chiave</span><span class="sxs-lookup"><span data-stu-id="1ebbd-271">Key</span></span>       | <span data-ttu-id="1ebbd-272">Value</span><span class="sxs-lookup"><span data-stu-id="1ebbd-272">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="1ebbd-273">Se le chiavi con mapping di sostituzione vengono usate all'avvio dell'app, la configurazione riceve il valore di configurazione per la chiave fornita dal dizionario:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-273">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="1ebbd-274">Dopo aver eseguito il comando precedente, la configurazione contiene i valori mostrati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-274">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="1ebbd-275">Chiave</span><span class="sxs-lookup"><span data-stu-id="1ebbd-275">Key</span></span>               | <span data-ttu-id="1ebbd-276">Value</span><span class="sxs-lookup"><span data-stu-id="1ebbd-276">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="1ebbd-277">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="1ebbd-277">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="1ebbd-278"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> carica la configurazione da coppie chiave-valore di variabili di ambiente in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-278">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="1ebbd-279">Per attivare la configurazione delle variabili di ambiente, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-279">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="1ebbd-280">[Servizio App di Azure](https://azure.microsoft.com/services/app-service/) consente di impostare variabili di ambiente nel portale di Azure che possono sostituire la configurazione delle app tramite il provider di configurazione delle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-280">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="1ebbd-281">Per altre informazioni, vedere [App Azure: Eseguire l'override della configurazione delle app usando il portale di Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-281">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="1ebbd-282">`AddEnvironmentVariables` viene chiamato automaticamente per le variabili di ambiente con prefisso `ASPNETCORE_` quando si inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-282">`AddEnvironmentVariables` is automatically called for environment variables prefixed with `ASPNETCORE_` when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="1ebbd-283">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-283">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="1ebbd-284">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-284">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="1ebbd-285">Configurazione delle app dalle variabili di ambiente senza prefisso chiamando `AddEnvironmentVariables` senza prefisso.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-285">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="1ebbd-286">Una configurazione facoltativa da *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-286">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="1ebbd-287">[Segreti utente (Secret Manager)](xref:security/app-secrets) (nell'ambiente Development).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-287">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="1ebbd-288">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-288">Command-line arguments.</span></span>

<span data-ttu-id="1ebbd-289">Il provider di configurazione delle variabili di ambiente viene chiamato dopo aver stabilito la configurazione dai segreti utente e dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-289">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="1ebbd-290">La chiamata del provider in questa posizione consente alle variabili di ambiente lette in fase di esecuzione di sostituire la configurazione impostata dai segreti utente e dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-290">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="1ebbd-291">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-291">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="1ebbd-292">Se è necessario specificare la configurazione dell'app da variabili di ambiente aggiuntive, chiamare i provider aggiuntivi dell'app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> e chiamare `AddEnvironmentVariables` con il prefisso.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-292">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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
                // Call AddEnvironmentVariables last if you need to allow environment
                // variables to override values from other providers.
                config.AddEnvironmentVariables(prefix: "PREFIX_");
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="1ebbd-293">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-293">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="1ebbd-294">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="1ebbd-294">**Example**</span></span>

<span data-ttu-id="1ebbd-295">L'app di esempio consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include una chiamata a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-295">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="1ebbd-296">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-296">Run the sample app.</span></span> <span data-ttu-id="1ebbd-297">Aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-297">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="1ebbd-298">Notare che l'output contiene la coppia chiave-valore per la variabile di ambiente `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-298">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="1ebbd-299">Il valore riflette l'ambiente in cui l'app è in esecuzione, in genere `Development` durante l'esecuzione locale.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-299">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="1ebbd-300">Per mantenere breve l'elenco delle variabili di ambiente restituito dall'app, l'app filtra le variabili di ambiente includendo quelle che iniziano come segue:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-300">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="1ebbd-301">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="1ebbd-301">ASPNETCORE_</span></span>
* <span data-ttu-id="1ebbd-302">urls</span><span class="sxs-lookup"><span data-stu-id="1ebbd-302">urls</span></span>
* <span data-ttu-id="1ebbd-303">Registrazione</span><span class="sxs-lookup"><span data-stu-id="1ebbd-303">Logging</span></span>
* <span data-ttu-id="1ebbd-304">ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="1ebbd-304">ENVIRONMENT</span></span>
* <span data-ttu-id="1ebbd-305">contentRoot</span><span class="sxs-lookup"><span data-stu-id="1ebbd-305">contentRoot</span></span>
* <span data-ttu-id="1ebbd-306">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="1ebbd-306">AllowedHosts</span></span>
* <span data-ttu-id="1ebbd-307">applicationName</span><span class="sxs-lookup"><span data-stu-id="1ebbd-307">applicationName</span></span>
* <span data-ttu-id="1ebbd-308">CommandLine</span><span class="sxs-lookup"><span data-stu-id="1ebbd-308">CommandLine</span></span>

<span data-ttu-id="1ebbd-309">Se si desidera esporre tutte le variabili di ambiente disponibili all'app, modificare `FilteredConfiguration` in *Pages/Index.cshtml.cs* come segue:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-309">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="1ebbd-310">Prefissi</span><span class="sxs-lookup"><span data-stu-id="1ebbd-310">Prefixes</span></span>

<span data-ttu-id="1ebbd-311">Le variabili di ambiente caricate nella configurazione dell'app vengono filtrate quando si specifica un prefisso per il metodo `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-311">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="1ebbd-312">Ad esempio, per filtrare le variabili di ambiente in base al prefisso `CUSTOM_`, fornire il prefisso al provider di configurazione:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-312">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="1ebbd-313">Il prefisso viene rimosso quando vengono create le coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-313">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="1ebbd-314">Il metodo di servizio statico `CreateDefaultBuilder` crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> per stabilire l'host dell'app.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-314">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="1ebbd-315">Quando viene creato `WebHostBuilder`, la configurazione dell'host corrispondente viene individuata nelle variabili di ambiente con il prefisso `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-315">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

<span data-ttu-id="1ebbd-316">**Prefissi della stringa di connessione**</span><span class="sxs-lookup"><span data-stu-id="1ebbd-316">**Connection string prefixes**</span></span>

<span data-ttu-id="1ebbd-317">L'API di configurazione prevede regole di elaborazione speciali per quattro variabili di ambiente di stringa di connessione coinvolte nella configurazione di stringhe di connessione di Azure per l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-317">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="1ebbd-318">Le variabili di ambiente con i prefissi indicati nella tabella vengono caricate nell'app se non viene fornito alcun prefisso a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-318">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="1ebbd-319">Prefisso della stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="1ebbd-319">Connection string prefix</span></span> | <span data-ttu-id="1ebbd-320">Provider</span><span class="sxs-lookup"><span data-stu-id="1ebbd-320">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="1ebbd-321">Provider personalizzato</span><span class="sxs-lookup"><span data-stu-id="1ebbd-321">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="1ebbd-322">MySQL</span><span class="sxs-lookup"><span data-stu-id="1ebbd-322">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="1ebbd-323">Database SQL di Azure</span><span class="sxs-lookup"><span data-stu-id="1ebbd-323">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="1ebbd-324">SQL Server</span><span class="sxs-lookup"><span data-stu-id="1ebbd-324">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="1ebbd-325">Quando una variabile di ambiente viene individuata e caricata nella configurazione con uno qualsiasi dei quattro prefissi indicati nella tabella:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-325">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="1ebbd-326">La chiave di configurazione viene creata rimuovendo il prefisso della variabile di ambiente e aggiungendo una sezione per la chiave di configurazione (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-326">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="1ebbd-327">Viene creata una nuova coppia chiave-valore della configurazione che rappresenta il provider di connessione al database (ad eccezione di `CUSTOMCONNSTR_`, che non ha un provider dichiarato).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-327">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="1ebbd-328">Chiave della variabile di ambiente</span><span class="sxs-lookup"><span data-stu-id="1ebbd-328">Environment variable key</span></span> | <span data-ttu-id="1ebbd-329">Chiave di configurazione convertita</span><span class="sxs-lookup"><span data-stu-id="1ebbd-329">Converted configuration key</span></span> | <span data-ttu-id="1ebbd-330">Voce di configurazione del provider</span><span class="sxs-lookup"><span data-stu-id="1ebbd-330">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="1ebbd-331">Voce di configurazione non creata.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-331">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="1ebbd-332">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-332">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="1ebbd-333">Valore: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="1ebbd-333">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="1ebbd-334">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-334">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="1ebbd-335">Valore: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="1ebbd-335">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="1ebbd-336">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-336">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="1ebbd-337">Valore: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="1ebbd-337">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="1ebbd-338">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="1ebbd-338">File Configuration Provider</span></span>

<span data-ttu-id="1ebbd-339"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> è la classe base per il caricamento della configurazione dal file system.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-339"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="1ebbd-340">I provider di configurazione seguenti sono dedicati per specifici tipi di file:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-340">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="1ebbd-341">Provider di configurazione INI</span><span class="sxs-lookup"><span data-stu-id="1ebbd-341">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="1ebbd-342">Provider di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="1ebbd-342">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="1ebbd-343">Provider di configurazione XML</span><span class="sxs-lookup"><span data-stu-id="1ebbd-343">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="1ebbd-344">Provider di configurazione INI</span><span class="sxs-lookup"><span data-stu-id="1ebbd-344">INI Configuration Provider</span></span>

<span data-ttu-id="1ebbd-345"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> carica la configurazione da coppie chiave-valore di file INI in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-345">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="1ebbd-346">Per attivare la configurazione di file INI, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-346">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1ebbd-347">I due punti possono essere usati come delimitatore di sezione nella configurazione di file INI.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-347">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="1ebbd-348">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-348">Overloads permit specifying:</span></span>

* <span data-ttu-id="1ebbd-349">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-349">Whether the file is optional.</span></span>
* <span data-ttu-id="1ebbd-350">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-350">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="1ebbd-351">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-351">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="1ebbd-352">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-352">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddIniFile("config.ini", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="1ebbd-353">Il percorso di base è impostato con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-353">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="1ebbd-354">`SetBasePath` è incluso nel pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-354">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="1ebbd-355">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-355">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="1ebbd-356">Il percorso di base è impostato con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-356">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="1ebbd-357">`SetBasePath` è incluso nel pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-357">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="1ebbd-358">Un esempio generico di un file di configurazione INI:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-358">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="1ebbd-359">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-359">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="1ebbd-360">section0:key0</span><span class="sxs-lookup"><span data-stu-id="1ebbd-360">section0:key0</span></span>
* <span data-ttu-id="1ebbd-361">section0:key1</span><span class="sxs-lookup"><span data-stu-id="1ebbd-361">section0:key1</span></span>
* <span data-ttu-id="1ebbd-362">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="1ebbd-362">section1:subsection:key</span></span>
* <span data-ttu-id="1ebbd-363">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="1ebbd-363">section2:subsection0:key</span></span>
* <span data-ttu-id="1ebbd-364">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="1ebbd-364">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="1ebbd-365">Provider di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="1ebbd-365">JSON Configuration Provider</span></span>

<span data-ttu-id="1ebbd-366">Il <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> carica la configurazione da coppie chiave-valore di file JSON in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-366">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="1ebbd-367">Per attivare la configurazione di file JSON, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-367">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1ebbd-368">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-368">Overloads permit specifying:</span></span>

* <span data-ttu-id="1ebbd-369">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-369">Whether the file is optional.</span></span>
* <span data-ttu-id="1ebbd-370">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-370">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="1ebbd-371">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-371">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="1ebbd-372">`AddJsonFile` viene chiamato automaticamente due volte quando si inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-372">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="1ebbd-373">Il metodo viene chiamato per caricare la configurazione da:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-373">The method is called to load configuration from:</span></span>

* <span data-ttu-id="1ebbd-374">*appsettings.json* &ndash; Questo file viene letto per primo.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-374">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="1ebbd-375">La versione dell'ambiente del file può sostituire i valori forniti dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-375">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="1ebbd-376">*appsettings.{Environment}.json* &ndash; La versione dell'ambiente del file viene caricata in base a [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-376">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="1ebbd-377">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-377">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="1ebbd-378">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-378">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="1ebbd-379">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-379">Environment variables.</span></span>
* <span data-ttu-id="1ebbd-380">[Segreti utente (Secret Manager)](xref:security/app-secrets) (nell'ambiente Development).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-380">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="1ebbd-381">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-381">Command-line arguments.</span></span>

<span data-ttu-id="1ebbd-382">Il provider di configurazione JSON viene stabilito per primo.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-382">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="1ebbd-383">I segreti utente, le variabili di ambiente e gli argomenti della riga di comando sostituiscono quindi la configurazione impostata dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-383">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="1ebbd-384">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app per file diversi da *appsettings.json* e *appsettings.{Environment}.json*:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-384">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

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
                config.AddJsonFile("config.json", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="1ebbd-385">Il percorso di base è impostato con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-385">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="1ebbd-386">`SetBasePath` è incluso nel pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-386">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="1ebbd-387">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-387">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="1ebbd-388">Il percorso di base è impostato con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-388">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="1ebbd-389">`SetBasePath` è incluso nel pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-389">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="1ebbd-390">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="1ebbd-390">**Example**</span></span>

<span data-ttu-id="1ebbd-391">L'app di esempio consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include due chiamate a `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-391">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="1ebbd-392">La configurazione viene caricata da *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-392">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

1. <span data-ttu-id="1ebbd-393">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-393">Run the sample app.</span></span> <span data-ttu-id="1ebbd-394">Aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-394">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="1ebbd-395">Notare che l'output contiene coppie chiave-valore per la configurazione mostrata nella tabella in base all'ambiente.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-395">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="1ebbd-396">Le chiavi di configurazione di registrazione usano i due punti (`:`) come separatore gerarchico.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-396">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="1ebbd-397">Chiave</span><span class="sxs-lookup"><span data-stu-id="1ebbd-397">Key</span></span>                        | <span data-ttu-id="1ebbd-398">Valore Development</span><span class="sxs-lookup"><span data-stu-id="1ebbd-398">Development Value</span></span> | <span data-ttu-id="1ebbd-399">Valore Production</span><span class="sxs-lookup"><span data-stu-id="1ebbd-399">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="1ebbd-400">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="1ebbd-400">Logging:LogLevel:System</span></span>    | <span data-ttu-id="1ebbd-401">Informazioni</span><span class="sxs-lookup"><span data-stu-id="1ebbd-401">Information</span></span>       | <span data-ttu-id="1ebbd-402">Informazioni</span><span class="sxs-lookup"><span data-stu-id="1ebbd-402">Information</span></span>      |
| <span data-ttu-id="1ebbd-403">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="1ebbd-403">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="1ebbd-404">Informazioni</span><span class="sxs-lookup"><span data-stu-id="1ebbd-404">Information</span></span>       | <span data-ttu-id="1ebbd-405">Informazioni</span><span class="sxs-lookup"><span data-stu-id="1ebbd-405">Information</span></span>      |
| <span data-ttu-id="1ebbd-406">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="1ebbd-406">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="1ebbd-407">Debug</span><span class="sxs-lookup"><span data-stu-id="1ebbd-407">Debug</span></span>             | <span data-ttu-id="1ebbd-408">Error</span><span class="sxs-lookup"><span data-stu-id="1ebbd-408">Error</span></span>            |
| <span data-ttu-id="1ebbd-409">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="1ebbd-409">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="1ebbd-410">Provider di configurazione XML</span><span class="sxs-lookup"><span data-stu-id="1ebbd-410">XML Configuration Provider</span></span>

<span data-ttu-id="1ebbd-411">Il <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> carica la configurazione da coppie chiave-valore di file XML in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-411">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="1ebbd-412">Per attivare la configurazione di file XML, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-412">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1ebbd-413">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-413">Overloads permit specifying:</span></span>

* <span data-ttu-id="1ebbd-414">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-414">Whether the file is optional.</span></span>
* <span data-ttu-id="1ebbd-415">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-415">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="1ebbd-416">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-416">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="1ebbd-417">Il nodo radice del file di configurazione viene ignorato quando vengono create le coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-417">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="1ebbd-418">Non specificare una definizione DTD (Document Type Definition) o uno spazio dei nomi nel file.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-418">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="1ebbd-419">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-419">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddXmlFile("config.xml", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="1ebbd-420">Il percorso di base è impostato con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-420">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="1ebbd-421">`SetBasePath` è incluso nel pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-421">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="1ebbd-422">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-422">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="1ebbd-423">Il percorso di base è impostato con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-423">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="1ebbd-424">`SetBasePath` è incluso nel pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-424">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="1ebbd-425">I file di configurazione XML possono usare nomi di elementi distinti per le sezioni ripetute:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-425">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="1ebbd-426">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-426">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="1ebbd-427">section0:key0</span><span class="sxs-lookup"><span data-stu-id="1ebbd-427">section0:key0</span></span>
* <span data-ttu-id="1ebbd-428">section0:key1</span><span class="sxs-lookup"><span data-stu-id="1ebbd-428">section0:key1</span></span>
* <span data-ttu-id="1ebbd-429">section1:key0</span><span class="sxs-lookup"><span data-stu-id="1ebbd-429">section1:key0</span></span>
* <span data-ttu-id="1ebbd-430">section1:key1</span><span class="sxs-lookup"><span data-stu-id="1ebbd-430">section1:key1</span></span>

<span data-ttu-id="1ebbd-431">La ripetizione di elementi che usano lo stesso nome di elemento funziona se si usa l'attributo `name` per distinguere gli elementi:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-431">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="1ebbd-432">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-432">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="1ebbd-433">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="1ebbd-433">section:section0:key:key0</span></span>
* <span data-ttu-id="1ebbd-434">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="1ebbd-434">section:section0:key:key1</span></span>
* <span data-ttu-id="1ebbd-435">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="1ebbd-435">section:section1:key:key0</span></span>
* <span data-ttu-id="1ebbd-436">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="1ebbd-436">section:section1:key:key1</span></span>

<span data-ttu-id="1ebbd-437">È possibile usare attributi per fornire valori:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-437">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="1ebbd-438">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-438">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="1ebbd-439">key:attribute</span><span class="sxs-lookup"><span data-stu-id="1ebbd-439">key:attribute</span></span>
* <span data-ttu-id="1ebbd-440">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="1ebbd-440">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="1ebbd-441">Provider di configurazione KeyPerFile</span><span class="sxs-lookup"><span data-stu-id="1ebbd-441">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="1ebbd-442">Il <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> usa i file di una directory come coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-442">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="1ebbd-443">La chiave è il nome del file.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-443">The key is the file name.</span></span> <span data-ttu-id="1ebbd-444">Il valore contiene il contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-444">The value contains the file's contents.</span></span> <span data-ttu-id="1ebbd-445">Il provider di configurazione KeyPerFile viene usato in scenari di hosting Docker.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-445">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="1ebbd-446">Per attivare la configurazione chiave-per-file, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-446">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="1ebbd-447">Il `directoryPath` per i file deve essere un percorso assoluto.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-447">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="1ebbd-448">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-448">Overloads permit specifying:</span></span>

* <span data-ttu-id="1ebbd-449">Un delegato `Action<KeyPerFileConfigurationSource>` che configura l'origine.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-449">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="1ebbd-450">Se la directory è facoltativa e il percorso della directory.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-450">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="1ebbd-451">Il doppio carattere di sottolineatura (`__`) viene usato come delimitatore per le chiavi di configurazione nei nomi dei file.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-451">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="1ebbd-452">Ad esempio, il nome di file `Logging__LogLevel__System` produce la chiave di configurazione `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-452">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="1ebbd-453">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-453">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
                config.AddKeyPerFile(directoryPath: path, optional: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="1ebbd-454">Il percorso di base è impostato con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-454">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="1ebbd-455">`SetBasePath` è incluso nel pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-455">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="1ebbd-456">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-456">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="1ebbd-457">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="1ebbd-457">Memory Configuration Provider</span></span>

<span data-ttu-id="1ebbd-458">Il <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> usa una raccolta in memoria come coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-458">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="1ebbd-459">Per attivare la configurazione della raccolta in memoria, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-459">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="1ebbd-460">Il provider di configurazione può essere inizializzato con un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-460">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="1ebbd-461">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-461">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="1ebbd-462">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-462">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="1ebbd-463">GetValue</span><span class="sxs-lookup"><span data-stu-id="1ebbd-463">GetValue</span></span>

<span data-ttu-id="1ebbd-464">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) estrae un valore dalla configurazione con una chiave specificata e lo converte nel tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-464">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="1ebbd-465">Un overload consente di specificare un valore predefinito se non viene trovata la chiave.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-465">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="1ebbd-466">L'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-466">The following example:</span></span>

* <span data-ttu-id="1ebbd-467">Estrae il valore di stringa dalla configurazione con la chiave `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-467">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="1ebbd-468">Se `NumberKey` non viene trovato nelle chiavi di configurazione, viene usato il valore predefinito di `99`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-468">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="1ebbd-469">Assegna al valore il tipo `int`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-469">Types the value as an `int`.</span></span>
* <span data-ttu-id="1ebbd-470">Memorizza il valore nella proprietà `NumberConfig` per l'uso da parte della pagina.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-470">Stores the value in the `NumberConfig` property for use by the page.</span></span>

```csharp
// using Microsoft.Extensions.Configuration;

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="1ebbd-471">GetSection, GetChildren ed Exists</span><span class="sxs-lookup"><span data-stu-id="1ebbd-471">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="1ebbd-472">Per gli esempi che seguono, prendere in considerazione il file JSON seguente.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-472">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="1ebbd-473">Vengono trovate quattro chiavi in due sezioni, una delle quali include una coppia di sottosezioni:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-473">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="1ebbd-474">Quando il file viene letto nella configurazione, vengono create le chiavi gerarchiche univoche seguenti per contenere i valori di configurazione:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-474">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="1ebbd-475">section0:key0</span><span class="sxs-lookup"><span data-stu-id="1ebbd-475">section0:key0</span></span>
* <span data-ttu-id="1ebbd-476">section0:key1</span><span class="sxs-lookup"><span data-stu-id="1ebbd-476">section0:key1</span></span>
* <span data-ttu-id="1ebbd-477">section1:key0</span><span class="sxs-lookup"><span data-stu-id="1ebbd-477">section1:key0</span></span>
* <span data-ttu-id="1ebbd-478">section1:key1</span><span class="sxs-lookup"><span data-stu-id="1ebbd-478">section1:key1</span></span>
* <span data-ttu-id="1ebbd-479">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="1ebbd-479">section2:subsection0:key0</span></span>
* <span data-ttu-id="1ebbd-480">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="1ebbd-480">section2:subsection0:key1</span></span>
* <span data-ttu-id="1ebbd-481">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="1ebbd-481">section2:subsection1:key0</span></span>
* <span data-ttu-id="1ebbd-482">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="1ebbd-482">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="1ebbd-483">GetSection</span><span class="sxs-lookup"><span data-stu-id="1ebbd-483">GetSection</span></span>

<span data-ttu-id="1ebbd-484">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) estrae una sottosezione della configurazione con la chiave di sottosezione specificata.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-484">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span> <span data-ttu-id="1ebbd-485">`GetSection` è incluso nel pacchetto [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-485">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="1ebbd-486">Per restituire una <xref:Microsoft.Extensions.Configuration.IConfigurationSection> che contiene solo le coppie chiave-valore in `section1`, chiamare `GetSection` e fornire il nome della sezione:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-486">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="1ebbd-487">`configSection` non ha un valore, ma solo una chiave e un percorso.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-487">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="1ebbd-488">In modo analogo, per ottenere i valori per le chiavi in `section2:subsection0`, chiamare `GetSection` e fornire il percorso della sezione:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-488">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="1ebbd-489">`GetSection` non restituisce mai `null`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-489">`GetSection` never returns `null`.</span></span> <span data-ttu-id="1ebbd-490">Se non viene trovata una sezione corrispondente, viene restituita una `IConfigurationSection` vuota.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-490">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="1ebbd-491">Quando `GetSection` restituisce una sezione corrispondente, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> non viene compilata.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-491">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="1ebbd-492">Quando la sezione esiste, vengono restituiti <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> e <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-492">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="1ebbd-493">GetChildren</span><span class="sxs-lookup"><span data-stu-id="1ebbd-493">GetChildren</span></span>

<span data-ttu-id="1ebbd-494">Una chiamata a [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) per `section2` ottiene una `IEnumerable<IConfigurationSection>` che include:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-494">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="1ebbd-495">Esistente</span><span class="sxs-lookup"><span data-stu-id="1ebbd-495">Exists</span></span>

<span data-ttu-id="1ebbd-496">Usare [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) per determinare se esiste una sezione di configurazione:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-496">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="1ebbd-497">Considerati i dati di esempio, `sectionExists` è `false` perché non esiste una sezione `section2:subsection2` nei dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-497">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="1ebbd-498">Eseguire l'associazione a una classe</span><span class="sxs-lookup"><span data-stu-id="1ebbd-498">Bind to a class</span></span>

<span data-ttu-id="1ebbd-499">La configurazione può essere associata alle classi che rappresentano gruppi di impostazioni correlate usando il *modello di opzioni*.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-499">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="1ebbd-500">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-500">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="1ebbd-501">I valori di configurazione vengono restituiti come stringhe, ma la chiamata di <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> consente la costruzione di oggetti [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-501">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="1ebbd-502">`Bind` è incluso nel pacchetto [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-502">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="1ebbd-503">L'app di esempio contiene un modello `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="1ebbd-503">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="1ebbd-504">La sezione `starship` del file *starship.json* crea la configurazione quando l'app di esempio usa il provider di configurazione JSON per caricare la configurazione:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-504">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

<span data-ttu-id="1ebbd-505">Vengono create le coppie chiave-valore della configurazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-505">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="1ebbd-506">Chiave</span><span class="sxs-lookup"><span data-stu-id="1ebbd-506">Key</span></span>                   | <span data-ttu-id="1ebbd-507">Value</span><span class="sxs-lookup"><span data-stu-id="1ebbd-507">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="1ebbd-508">starship:name</span><span class="sxs-lookup"><span data-stu-id="1ebbd-508">starship:name</span></span>         | <span data-ttu-id="1ebbd-509">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="1ebbd-509">USS Enterprise</span></span>                                    |
| <span data-ttu-id="1ebbd-510">starship:registry</span><span class="sxs-lookup"><span data-stu-id="1ebbd-510">starship:registry</span></span>     | <span data-ttu-id="1ebbd-511">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="1ebbd-511">NCC-1701</span></span>                                          |
| <span data-ttu-id="1ebbd-512">starship:class</span><span class="sxs-lookup"><span data-stu-id="1ebbd-512">starship:class</span></span>        | <span data-ttu-id="1ebbd-513">Constitution</span><span class="sxs-lookup"><span data-stu-id="1ebbd-513">Constitution</span></span>                                      |
| <span data-ttu-id="1ebbd-514">starship:length</span><span class="sxs-lookup"><span data-stu-id="1ebbd-514">starship:length</span></span>       | <span data-ttu-id="1ebbd-515">304.8</span><span class="sxs-lookup"><span data-stu-id="1ebbd-515">304.8</span></span>                                             |
| <span data-ttu-id="1ebbd-516">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="1ebbd-516">starship:commissioned</span></span> | <span data-ttu-id="1ebbd-517">False</span><span class="sxs-lookup"><span data-stu-id="1ebbd-517">False</span></span>                                             |
| <span data-ttu-id="1ebbd-518">trademark</span><span class="sxs-lookup"><span data-stu-id="1ebbd-518">trademark</span></span>             | <span data-ttu-id="1ebbd-519">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="1ebbd-519">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="1ebbd-520">L'app di esempio chiama `GetSection` con la chiave `starship`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-520">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="1ebbd-521">Le coppie chiave-valore `starship` sono isolate.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-521">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="1ebbd-522">Il metodo `Bind` viene chiamato sulla sottosezione passando un'istanza della classe `Starship`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-522">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="1ebbd-523">Dopo aver associato i valori dell'istanza, l'istanza viene assegnata a una proprietà per il rendering:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-523">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

<span data-ttu-id="1ebbd-524">`GetSection` è incluso nel pacchetto [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-524">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="1ebbd-525">Associazione a un oggetto grafico</span><span class="sxs-lookup"><span data-stu-id="1ebbd-525">Bind to an object graph</span></span>

<span data-ttu-id="1ebbd-526"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> è in grado di associare un intero oggetto grafico POCO.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-526"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="1ebbd-527">`Bind` è incluso nel pacchetto [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-527">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="1ebbd-528">L'esempio contiene un modello `TvShow` il cui oggetto grafico include `Metadata` e le classi `Actors` (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="1ebbd-528">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="1ebbd-529">L'app di esempio ha un file *tvshow.xml* che contiene i dati di configurazione:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-529">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="1ebbd-530">La configurazione viene associata all'intero oggetto grafico `TvShow` con il metodo `Bind`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-530">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="1ebbd-531">L'istanza associata viene assegnata a una proprietà per il rendering:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-531">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="1ebbd-532">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) associa e restituisce il tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-532">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="1ebbd-533">`Get<T>` può essere più comodo che usare `Bind`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-533">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="1ebbd-534">Il codice seguente mostra come usare `Get<T>` con l'esempio precedente, che consente di assegnare l'istanza associata direttamente alla proprietà usata per il rendering:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-534">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

<span data-ttu-id="1ebbd-535"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> è incluso nel pacchetto [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-535"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="1ebbd-536">`Get<T>` è disponibile in ASP.NET Core 1.1 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-536">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="1ebbd-537">`GetSection` è incluso nel pacchetto [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-537">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="1ebbd-538">Associare una matrice a una classe</span><span class="sxs-lookup"><span data-stu-id="1ebbd-538">Bind an array to a class</span></span>

<span data-ttu-id="1ebbd-539">*L'app di esempio dimostra i concetti spiegati in questa sezione.*</span><span class="sxs-lookup"><span data-stu-id="1ebbd-539">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="1ebbd-540">Il <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supporta l'associazione di matrici agli oggetti usando gli indici delle matrici nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-540">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="1ebbd-541">Qualsiasi formato di matrice che espone un segmento chiave numerico (`:0:`, `:1:`, &hellip; `:{n}:`) supporta l'associazione di matrici a una matrice di classi POCO.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-541">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span> <span data-ttu-id="1ebbd-542">"Bind" è incluso nel pacchetto [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-542">\`Bind\`\` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

> [!NOTE]
> <span data-ttu-id="1ebbd-543">L'associazione viene fornita per convenzione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-543">Binding is provided by convention.</span></span> <span data-ttu-id="1ebbd-544">I provider di configurazione personalizzati non devono implementare l'associazione di matrici.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-544">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="1ebbd-545">**Elaborazione di matrici in memoria**</span><span class="sxs-lookup"><span data-stu-id="1ebbd-545">**In-memory array processing**</span></span>

<span data-ttu-id="1ebbd-546">Prendere in considerazione le chiavi di configurazione e i valori indicati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-546">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="1ebbd-547">Chiave</span><span class="sxs-lookup"><span data-stu-id="1ebbd-547">Key</span></span>             | <span data-ttu-id="1ebbd-548">Value</span><span class="sxs-lookup"><span data-stu-id="1ebbd-548">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="1ebbd-549">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="1ebbd-549">array:entries:0</span></span> | <span data-ttu-id="1ebbd-550">value0</span><span class="sxs-lookup"><span data-stu-id="1ebbd-550">value0</span></span> |
| <span data-ttu-id="1ebbd-551">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="1ebbd-551">array:entries:1</span></span> | <span data-ttu-id="1ebbd-552">value1</span><span class="sxs-lookup"><span data-stu-id="1ebbd-552">value1</span></span> |
| <span data-ttu-id="1ebbd-553">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="1ebbd-553">array:entries:2</span></span> | <span data-ttu-id="1ebbd-554">value2</span><span class="sxs-lookup"><span data-stu-id="1ebbd-554">value2</span></span> |
| <span data-ttu-id="1ebbd-555">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="1ebbd-555">array:entries:4</span></span> | <span data-ttu-id="1ebbd-556">value4</span><span class="sxs-lookup"><span data-stu-id="1ebbd-556">value4</span></span> |
| <span data-ttu-id="1ebbd-557">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="1ebbd-557">array:entries:5</span></span> | <span data-ttu-id="1ebbd-558">value5</span><span class="sxs-lookup"><span data-stu-id="1ebbd-558">value5</span></span> |

<span data-ttu-id="1ebbd-559">Queste chiavi e i valori vengono caricati nell'app di esempio usando il provider di configurazione della memoria:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-559">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

<span data-ttu-id="1ebbd-560">La matrice ignora un valore per l'indice &num;3.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-560">The array skips a value for index &num;3.</span></span> <span data-ttu-id="1ebbd-561">Lo strumento di associazione di configurazione non è in grado di associare valori null o di creare voci null negli oggetti associati, situazione che diventerà chiara tra poco quando viene illustrato il risultato dell'associazione di questa matrice a un oggetto.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-561">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="1ebbd-562">Nell'app di esempio, è disponibile una classe POCO per i dati di configurazione associati:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-562">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="1ebbd-563">I dati di configurazione sono associati all'oggetto:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-563">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="1ebbd-564">`GetSection` è incluso nel pacchetto [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-564">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="1ebbd-565">È possibile usare anche la sintassi [ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*), che produce codice più compatto:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-565">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="1ebbd-566">L'oggetto associato, un'istanza di `ArrayExample`, riceve i dati di matrice dalla configurazione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-566">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="1ebbd-567">Indice di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="1ebbd-567">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="1ebbd-568">Valore di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="1ebbd-568">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="1ebbd-569">0</span><span class="sxs-lookup"><span data-stu-id="1ebbd-569">0</span></span>                            | <span data-ttu-id="1ebbd-570">value0</span><span class="sxs-lookup"><span data-stu-id="1ebbd-570">value0</span></span>                       |
| <span data-ttu-id="1ebbd-571">1</span><span class="sxs-lookup"><span data-stu-id="1ebbd-571">1</span></span>                            | <span data-ttu-id="1ebbd-572">value1</span><span class="sxs-lookup"><span data-stu-id="1ebbd-572">value1</span></span>                       |
| <span data-ttu-id="1ebbd-573">2</span><span class="sxs-lookup"><span data-stu-id="1ebbd-573">2</span></span>                            | <span data-ttu-id="1ebbd-574">value2</span><span class="sxs-lookup"><span data-stu-id="1ebbd-574">value2</span></span>                       |
| <span data-ttu-id="1ebbd-575">3</span><span class="sxs-lookup"><span data-stu-id="1ebbd-575">3</span></span>                            | <span data-ttu-id="1ebbd-576">value4</span><span class="sxs-lookup"><span data-stu-id="1ebbd-576">value4</span></span>                       |
| <span data-ttu-id="1ebbd-577">4</span><span class="sxs-lookup"><span data-stu-id="1ebbd-577">4</span></span>                            | <span data-ttu-id="1ebbd-578">value5</span><span class="sxs-lookup"><span data-stu-id="1ebbd-578">value5</span></span>                       |

<span data-ttu-id="1ebbd-579">L'indice &num;3 nell'oggetto associato contiene i dati di configurazione per la chiave di configurazione `array:4` e il relativo valore `value4`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-579">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="1ebbd-580">Quando i dati di configurazione contenenti una matrice vengono associati, gli indici di matrice nelle chiavi di configurazione vengono usati semplicemente per scorrere i dati di configurazione quando si crea l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-580">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="1ebbd-581">Un valore null non può essere mantenuto nei dati di configurazione e una voce con valore null non viene creata in un oggetto associato quando una matrice nelle chiavi di configurazione ignora uno o più indici.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-581">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="1ebbd-582">L'elemento di configurazione mancante per l'indice &num;3 può essere fornito prima dell'associazione all'istanza `ArrayExample` da qualsiasi provider di configurazione che produce la coppia chiave-valore corretta nella configurazione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-582">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="1ebbd-583">Se il codice di esempio include un provider di configurazione JSON aggiuntivo con la coppia chiave-valore mancante, `ArrayExample.Entries` corrisponde alla matrice di configurazione completa:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-583">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="1ebbd-584">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-584">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="1ebbd-585">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-585">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="1ebbd-586">La coppia chiave-valore mostrata nella tabella viene caricata nella configurazione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-586">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="1ebbd-587">Chiave</span><span class="sxs-lookup"><span data-stu-id="1ebbd-587">Key</span></span>             | <span data-ttu-id="1ebbd-588">Value</span><span class="sxs-lookup"><span data-stu-id="1ebbd-588">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="1ebbd-589">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="1ebbd-589">array:entries:3</span></span> | <span data-ttu-id="1ebbd-590">value3</span><span class="sxs-lookup"><span data-stu-id="1ebbd-590">value3</span></span> |

<span data-ttu-id="1ebbd-591">Se l'istanza della classe `ArrayExample` viene associata dopo che il provider di configurazione JSON include la voce per l'indice &num;3, la matrice `ArrayExample.Entries` include il valore.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-591">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="1ebbd-592">Indice di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="1ebbd-592">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="1ebbd-593">Valore di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="1ebbd-593">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="1ebbd-594">0</span><span class="sxs-lookup"><span data-stu-id="1ebbd-594">0</span></span>                            | <span data-ttu-id="1ebbd-595">value0</span><span class="sxs-lookup"><span data-stu-id="1ebbd-595">value0</span></span>                       |
| <span data-ttu-id="1ebbd-596">1</span><span class="sxs-lookup"><span data-stu-id="1ebbd-596">1</span></span>                            | <span data-ttu-id="1ebbd-597">value1</span><span class="sxs-lookup"><span data-stu-id="1ebbd-597">value1</span></span>                       |
| <span data-ttu-id="1ebbd-598">2</span><span class="sxs-lookup"><span data-stu-id="1ebbd-598">2</span></span>                            | <span data-ttu-id="1ebbd-599">value2</span><span class="sxs-lookup"><span data-stu-id="1ebbd-599">value2</span></span>                       |
| <span data-ttu-id="1ebbd-600">3</span><span class="sxs-lookup"><span data-stu-id="1ebbd-600">3</span></span>                            | <span data-ttu-id="1ebbd-601">value3</span><span class="sxs-lookup"><span data-stu-id="1ebbd-601">value3</span></span>                       |
| <span data-ttu-id="1ebbd-602">4</span><span class="sxs-lookup"><span data-stu-id="1ebbd-602">4</span></span>                            | <span data-ttu-id="1ebbd-603">value4</span><span class="sxs-lookup"><span data-stu-id="1ebbd-603">value4</span></span>                       |
| <span data-ttu-id="1ebbd-604">5</span><span class="sxs-lookup"><span data-stu-id="1ebbd-604">5</span></span>                            | <span data-ttu-id="1ebbd-605">value5</span><span class="sxs-lookup"><span data-stu-id="1ebbd-605">value5</span></span>                       |

<span data-ttu-id="1ebbd-606">**Elaborazione di matrice JSON**</span><span class="sxs-lookup"><span data-stu-id="1ebbd-606">**JSON array processing**</span></span>

<span data-ttu-id="1ebbd-607">Se un file JSON contiene una matrice, vengono create chiavi di configurazione per gli elementi della matrice con un indice di sezione in base zero.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-607">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="1ebbd-608">Nel file di configurazione seguente, `subsection` è una matrice:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-608">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="1ebbd-609">Il provider di configurazione JSON legge i dati di configurazione nelle coppie chiave-valore seguenti:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-609">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="1ebbd-610">Chiave</span><span class="sxs-lookup"><span data-stu-id="1ebbd-610">Key</span></span>                     | <span data-ttu-id="1ebbd-611">Value</span><span class="sxs-lookup"><span data-stu-id="1ebbd-611">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="1ebbd-612">json_array:key</span><span class="sxs-lookup"><span data-stu-id="1ebbd-612">json_array:key</span></span>          | <span data-ttu-id="1ebbd-613">valueA</span><span class="sxs-lookup"><span data-stu-id="1ebbd-613">valueA</span></span> |
| <span data-ttu-id="1ebbd-614">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="1ebbd-614">json_array:subsection:0</span></span> | <span data-ttu-id="1ebbd-615">valueB</span><span class="sxs-lookup"><span data-stu-id="1ebbd-615">valueB</span></span> |
| <span data-ttu-id="1ebbd-616">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="1ebbd-616">json_array:subsection:1</span></span> | <span data-ttu-id="1ebbd-617">valueC</span><span class="sxs-lookup"><span data-stu-id="1ebbd-617">valueC</span></span> |
| <span data-ttu-id="1ebbd-618">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="1ebbd-618">json_array:subsection:2</span></span> | <span data-ttu-id="1ebbd-619">valueD</span><span class="sxs-lookup"><span data-stu-id="1ebbd-619">valueD</span></span> |

<span data-ttu-id="1ebbd-620">Nell'app di esempio, la classe POCO seguente è disponibile per associare le coppie chiave-valore della configurazione:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-620">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="1ebbd-621">Dopo l'associazione, `JsonArrayExample.Key` contiene il valore `valueA`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-621">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="1ebbd-622">I valori di sottosezione vengono archiviati nella proprietà matrice POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-622">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="1ebbd-623">Indice di `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="1ebbd-623">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="1ebbd-624">Valore di `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="1ebbd-624">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="1ebbd-625">0</span><span class="sxs-lookup"><span data-stu-id="1ebbd-625">0</span></span>                                   | <span data-ttu-id="1ebbd-626">valueB</span><span class="sxs-lookup"><span data-stu-id="1ebbd-626">valueB</span></span>                              |
| <span data-ttu-id="1ebbd-627">1</span><span class="sxs-lookup"><span data-stu-id="1ebbd-627">1</span></span>                                   | <span data-ttu-id="1ebbd-628">valueC</span><span class="sxs-lookup"><span data-stu-id="1ebbd-628">valueC</span></span>                              |
| <span data-ttu-id="1ebbd-629">2</span><span class="sxs-lookup"><span data-stu-id="1ebbd-629">2</span></span>                                   | <span data-ttu-id="1ebbd-630">valueD</span><span class="sxs-lookup"><span data-stu-id="1ebbd-630">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="1ebbd-631">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="1ebbd-631">Custom configuration provider</span></span>

<span data-ttu-id="1ebbd-632">L'app di esempio dimostra come creare un provider di configurazione di base che legge le coppie chiave-valore di configurazione da un database usando [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-632">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="1ebbd-633">Il provider ha le caratteristiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-633">The provider has the following characteristics:</span></span>

* <span data-ttu-id="1ebbd-634">Il database in memoria di Entity Framework viene usato a scopo dimostrativo.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-634">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="1ebbd-635">Per usare un database che richiede una stringa di connessione, implementare un `ConfigurationBuilder` secondario per fornire la stringa di connessione da un altro provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-635">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="1ebbd-636">Il provider legge una tabella di database in una configurazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-636">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="1ebbd-637">Il provider non esegue una query sul database per ogni chiave.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-637">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="1ebbd-638">Il ricaricamento in caso di modifica non è implementato, quindi l'aggiornamento del database dopo l'avvio dell'app non ha alcun effetto sulla configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-638">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="1ebbd-639">Definire un'entità `EFConfigurationValue` per l'archiviazione dei valori di configurazione nel database.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-639">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="1ebbd-640">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-640">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="1ebbd-641">Aggiungere `EFConfigurationContext` per archiviare i valori configurati e accedervi.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-641">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="1ebbd-642">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-642">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="1ebbd-643">Creare una classe che implementi <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-643">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="1ebbd-644">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-644">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="1ebbd-645">Creare il provider di configurazione personalizzato ereditando da <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-645">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="1ebbd-646">Il provider di configurazione inizializza il database quando è vuoto.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-646">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="1ebbd-647">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-647">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="1ebbd-648">Un metodo di estensione `AddEFConfiguration` consente di aggiungere l'origine di configurazione a un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-648">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="1ebbd-649">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-649">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="1ebbd-650">L'esempio di codice seguente mostra come usare il `EFConfigurationProvider` personalizzato in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-650">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="1ebbd-651">Accedere alla configurazione durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="1ebbd-651">Access configuration during startup</span></span>

<span data-ttu-id="1ebbd-652">Inserire `IConfiguration` nel costruttore `Startup` per accedere ai valori di configurazione in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-652">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1ebbd-653">Per accedere alla configurazione in `Startup.Configure`, inserire `IConfiguration` direttamente nel metodo o usare l'istanza dal costruttore:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-653">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="1ebbd-654">Per un esempio di accesso alla configurazione usando metodi di servizio di avvio, vedere [Avvio dell'applicazione: Metodi pratici](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="1ebbd-654">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="1ebbd-655">Accedere alla configurazione in una pagina Razor Pages o in una visualizzazione MVC</span><span class="sxs-lookup"><span data-stu-id="1ebbd-655">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="1ebbd-656">Per accedere alle impostazioni di configurazione in una pagina Razor Pages o in una visualizzazione MVC, aggiungere un [direttiva using](xref:mvc/views/razor#using) ([Informazioni di riferimento su C#: direttiva using](/dotnet/csharp/language-reference/keywords/using-directive)) per lo [spazio dei nomi Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) e inserire <xref:Microsoft.Extensions.Configuration.IConfiguration> nella pagina o nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-656">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="1ebbd-657">In una pagina Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-657">In a Razor Pages page:</span></span>

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

<span data-ttu-id="1ebbd-658">In una visualizzazione MVC:</span><span class="sxs-lookup"><span data-stu-id="1ebbd-658">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="1ebbd-659">Aggiungere la configurazione da un assembly esterno</span><span class="sxs-lookup"><span data-stu-id="1ebbd-659">Add configuration from an external assembly</span></span>

<span data-ttu-id="1ebbd-660">Un'implementazione <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> consente l'aggiunta di miglioramenti a un'app all'avvio, da un assembly esterno alla classe `Startup` dell'app.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-660">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="1ebbd-661">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="1ebbd-661">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1ebbd-662">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="1ebbd-662">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* <span data-ttu-id="1ebbd-663">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/) (Configurazione Microsoft in dettaglio)</span><span class="sxs-lookup"><span data-stu-id="1ebbd-663">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)</span></span>
