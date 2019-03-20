---
title: Configurazione in ASP.NET Core
author: guardrex
description: Informazioni su come usare l'API di configurazione per configurare un'app ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/11/2019
uid: fundamentals/configuration/index
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="4d212-103">Configurazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4d212-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="4d212-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4d212-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4d212-105">La configurazione delle app in ASP.NET Core si basa su coppie chiave-valore stabilite dai *provider di configurazione*.</span><span class="sxs-lookup"><span data-stu-id="4d212-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="4d212-106">I provider di configurazione leggono i dati di configurazione in coppie chiave-valore da un'ampia gamma di origini di configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d212-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="4d212-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="4d212-107">Azure Key Vault</span></span>
* <span data-ttu-id="4d212-108">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="4d212-108">Command-line arguments</span></span>
* <span data-ttu-id="4d212-109">Provider personalizzati (installati o creati)</span><span class="sxs-lookup"><span data-stu-id="4d212-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="4d212-110">File della directory</span><span class="sxs-lookup"><span data-stu-id="4d212-110">Directory files</span></span>
* <span data-ttu-id="4d212-111">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="4d212-111">Environment variables</span></span>
* <span data-ttu-id="4d212-112">Oggetti .NET in memoria</span><span class="sxs-lookup"><span data-stu-id="4d212-112">In-memory .NET objects</span></span>
* <span data-ttu-id="4d212-113">File di impostazioni</span><span class="sxs-lookup"><span data-stu-id="4d212-113">Settings files</span></span>

<span data-ttu-id="4d212-114">Il *modello di opzioni* è un'estensione dei concetti di configurazione descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="4d212-114">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="4d212-115">Le opzioni usano le classi per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="4d212-115">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="4d212-116">Per altre informazioni sull'uso del modello di opzioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="4d212-116">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="4d212-117">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4d212-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4d212-118">Questi tre pacchetti sono inclusi nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4d212-118">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="host-vs-app-configuration"></a><span data-ttu-id="4d212-119">Host e configurazione delle app</span><span class="sxs-lookup"><span data-stu-id="4d212-119">Host vs. app configuration</span></span>

<span data-ttu-id="4d212-120">Prima che l'app venga configurata e avviata, viene configurato e avviato un *host*.</span><span class="sxs-lookup"><span data-stu-id="4d212-120">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="4d212-121">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="4d212-121">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="4d212-122">Sia l'app che l'host vengono configurati tramite i provider di configurazione descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="4d212-122">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="4d212-123">Le coppie chiave-valore di configurazione dell'host diventano parte della configurazione globale dell'app.</span><span class="sxs-lookup"><span data-stu-id="4d212-123">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="4d212-124">Per altre informazioni su come vengono usati i provider di configurazione per la creazione dell'host e sugli effetti delle origini di configurazione sulla configurazione dell'host, vedere [L'host](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="4d212-124">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see [The host](xref:fundamentals/index#host).</span></span>

## <a name="default-configuration"></a><span data-ttu-id="4d212-125">Configurazione predefinita</span><span class="sxs-lookup"><span data-stu-id="4d212-125">Default configuration</span></span>

<span data-ttu-id="4d212-126">Le app basate sui modelli [dotnet new](/dotnet/core/tools/dotnet-new) di ASP.NET Core chiamano <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> durante la compilazione di un host.</span><span class="sxs-lookup"><span data-stu-id="4d212-126">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="4d212-127"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> fornisce la configurazione predefinita per l'app nell'ordine seguente:</span><span class="sxs-lookup"><span data-stu-id="4d212-127"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> provides default configuration for the app in the following order:</span></span>

* <span data-ttu-id="4d212-128">La configurazione dell'host viene specificata da:</span><span class="sxs-lookup"><span data-stu-id="4d212-128">Host configuration is provided from:</span></span>
  * <span data-ttu-id="4d212-129">Variabili di ambiente con prefisso `ASPNETCORE_` (ad esempio `ASPNETCORE_ENVIRONMENT`) che usano il [provider di configurazione delle variabili di ambiente](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="4d212-129">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="4d212-130">Il prefisso (`ASPNETCORE_`) viene rimosso al caricamento delle coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d212-130">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="4d212-131">Argomenti della riga di comando che usano il [provider di configurazione della riga di comando](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="4d212-131">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="4d212-132">La configurazione dell'app viene fornita da:</span><span class="sxs-lookup"><span data-stu-id="4d212-132">App configuration is provided from:</span></span>
  * <span data-ttu-id="4d212-133">*appsettings.json* mediante il [provider di configurazione dei file](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="4d212-133">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="4d212-134">*appsettings.{Ambiente}.json* mediante il [provider di configurazione dei file](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="4d212-134">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="4d212-135">[Strumento di gestione dei segreti](xref:security/app-secrets) quando l'app viene eseguita nell'ambiente `Development` usando l'assembly di ingresso.</span><span class="sxs-lookup"><span data-stu-id="4d212-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="4d212-136">Variabili di ambiente che usano il [provider di configurazione delle variabili di ambiente](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="4d212-136">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="4d212-137">Se viene usato un prefisso personalizzato (ad esempio, `PREFIX_` con `.AddEnvironmentVariables(prefix: "PREFIX_")`), il prefisso viene rimosso al caricamento delle coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d212-137">If a custom prefix is used (for example, `PREFIX_` with `.AddEnvironmentVariables(prefix: "PREFIX_")`), the prefix is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="4d212-138">Argomenti della riga di comando che usano il [provider di configurazione della riga di comando](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="4d212-138">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="4d212-139">I provider di configurazione sono descritti più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="4d212-139">The configuration providers are explained later in this topic.</span></span> <span data-ttu-id="4d212-140">Per altre informazioni sull'host e su <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, vedere <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="4d212-140">For more information on the host and <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="security"></a><span data-ttu-id="4d212-141">Sicurezza</span><span class="sxs-lookup"><span data-stu-id="4d212-141">Security</span></span>

<span data-ttu-id="4d212-142">Adottare le procedure consigliate seguenti:</span><span class="sxs-lookup"><span data-stu-id="4d212-142">Adopt the following best practices:</span></span>

* <span data-ttu-id="4d212-143">Non archiviare mai la password o altri dati sensibili nel codice del provider di configurazione o in file di configurazione di testo normale.</span><span class="sxs-lookup"><span data-stu-id="4d212-143">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="4d212-144">Non usare i segreti di produzione in ambienti di sviluppo o di test.</span><span class="sxs-lookup"><span data-stu-id="4d212-144">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="4d212-145">Specificare i segreti all'esterno del progetto in modo che non possano essere inavvertitamente inviati a un repository del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="4d212-145">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="4d212-146">Altre informazioni su [come usare più ambienti](xref:fundamentals/environments) e la gestione dell'[archiviazione sicura dei segreti delle app durante lo sviluppo con Secret Manager](xref:security/app-secrets) (include consigli sull'uso delle variabili di ambiente per l'archiviazione di dati sensibili).</span><span class="sxs-lookup"><span data-stu-id="4d212-146">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="4d212-147">Secret Manager usa il provider di configurazione dei file per archiviare i segreti utente in un file JSON nel sistema locale.</span><span class="sxs-lookup"><span data-stu-id="4d212-147">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="4d212-148">Il provider di configurazione dei file è descritto più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="4d212-148">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="4d212-149">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) è una delle opzioni per l'archiviazione sicura dei segreti delle app.</span><span class="sxs-lookup"><span data-stu-id="4d212-149">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="4d212-150">Per ulteriori informazioni, vedere <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="4d212-150">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="4d212-151">Dati di configurazione gerarchici</span><span class="sxs-lookup"><span data-stu-id="4d212-151">Hierarchical configuration data</span></span>

<span data-ttu-id="4d212-152">L'API di configurazione è in grado di mantenere i dati di configurazione gerarchici rendendoli flat grazie all'uso di un delimitatore nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d212-152">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="4d212-153">Nel file JSON seguente, esistono quattro chiavi in una gerarchia strutturata di due sezioni:</span><span class="sxs-lookup"><span data-stu-id="4d212-153">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="4d212-154">Quando il file viene letto nella configurazione, vengono create chiavi univoche per mantenere la struttura di dati gerarchici originale dell'origine di configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d212-154">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="4d212-155">Le sezioni e le chiavi vengono rese flat usando due punti (`:`) per mantenere la struttura originale:</span><span class="sxs-lookup"><span data-stu-id="4d212-155">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="4d212-156">section0:key0</span><span class="sxs-lookup"><span data-stu-id="4d212-156">section0:key0</span></span>
* <span data-ttu-id="4d212-157">section0:key1</span><span class="sxs-lookup"><span data-stu-id="4d212-157">section0:key1</span></span>
* <span data-ttu-id="4d212-158">section1:key0</span><span class="sxs-lookup"><span data-stu-id="4d212-158">section1:key0</span></span>
* <span data-ttu-id="4d212-159">section1:key1</span><span class="sxs-lookup"><span data-stu-id="4d212-159">section1:key1</span></span>

<span data-ttu-id="4d212-160">I metodi <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> e <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> sono disponibili per isolare le sezioni e gli elementi figlio di una sezione nei dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d212-160"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="4d212-161">Questi metodi sono descritti più avanti in [GetSection, GetChildren ed Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="4d212-161">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span> <span data-ttu-id="4d212-162">`GetSection` è incluso nel pacchetto [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4d212-162">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="conventions"></a><span data-ttu-id="4d212-163">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="4d212-163">Conventions</span></span>

<span data-ttu-id="4d212-164">All'avvio dell'app, le origini di configurazione vengono lette nell'ordine con cui sono specificati i rispettivi provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d212-164">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="4d212-165">I provider di configurazione dei file supportano il ricaricamento della configurazione quando un file di impostazioni sottostante viene modificato dopo l'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="4d212-165">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="4d212-166">Il provider di configurazione dei file è descritto più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="4d212-166">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="4d212-167"><xref:Microsoft.Extensions.Configuration.IConfiguration> è disponibile nel contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) dell'app.</span><span class="sxs-lookup"><span data-stu-id="4d212-167"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="4d212-168"><xref:Microsoft.Extensions.Configuration.IConfiguration> può essere inserito in <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> di Razor Pages per ottenere la configurazione per la classe:</span><span class="sxs-lookup"><span data-stu-id="4d212-168"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

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

<span data-ttu-id="4d212-169">I provider di configurazione non possono usare l'inserimento delle dipendenze, perché non è disponibile quando vengono configurati dall'host.</span><span class="sxs-lookup"><span data-stu-id="4d212-169">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="4d212-170">Le chiavi di configurazione adottano le convenzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="4d212-170">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="4d212-171">Per le chiavi non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="4d212-171">Keys are case-insensitive.</span></span> <span data-ttu-id="4d212-172">Ad esempio, `ConnectionString` e `connectionstring` vengono considerate chiavi equivalenti.</span><span class="sxs-lookup"><span data-stu-id="4d212-172">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="4d212-173">Se un valore per la stessa chiave viene impostato dallo stesso provider di configurazione o da provider diversi, viene usato l'ultimo valore impostato per la chiave.</span><span class="sxs-lookup"><span data-stu-id="4d212-173">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="4d212-174">Chiavi gerarchiche</span><span class="sxs-lookup"><span data-stu-id="4d212-174">Hierarchical keys</span></span>
  * <span data-ttu-id="4d212-175">Nell'ambito dell'API di configurazione, il separatore due punti (`:`) funziona in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="4d212-175">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="4d212-176">Nelle variabili di ambiente, un separatore due punti potrebbe non funzionare in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="4d212-176">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="4d212-177">Il doppio carattere di sottolineatura (`__`) è supportato da tutte le piattaforme e viene convertito in due punti.</span><span class="sxs-lookup"><span data-stu-id="4d212-177">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="4d212-178">In Azure Key Vault, le chiavi gerarchiche usano `--` (due trattini) come separatore.</span><span class="sxs-lookup"><span data-stu-id="4d212-178">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="4d212-179">È necessario fornire il codice per sostituire i trattini con due punti quando i segreti vengono caricati nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="4d212-179">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="4d212-180">Il <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supporta l'associazione di matrici agli oggetti usando gli indici delle matrici nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d212-180">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="4d212-181">L'associazione di matrici è descritta nella sezione [Associare una matrice a una classe](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="4d212-181">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="4d212-182">I valori di configurazione adottano le convenzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="4d212-182">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="4d212-183">I valori sono stringhe.</span><span class="sxs-lookup"><span data-stu-id="4d212-183">Values are strings.</span></span>
* <span data-ttu-id="4d212-184">I valori null non possono essere archiviati nella configurazione o associati a oggetti.</span><span class="sxs-lookup"><span data-stu-id="4d212-184">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="4d212-185">Provider</span><span class="sxs-lookup"><span data-stu-id="4d212-185">Providers</span></span>

<span data-ttu-id="4d212-186">La tabella seguente mostra i provider di configurazione disponibili per le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4d212-186">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="4d212-187">Provider</span><span class="sxs-lookup"><span data-stu-id="4d212-187">Provider</span></span> | <span data-ttu-id="4d212-188">Fornisce la configurazione da&hellip;</span><span class="sxs-lookup"><span data-stu-id="4d212-188">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="4d212-189">[Provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="4d212-189">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="4d212-190">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="4d212-190">Azure Key Vault</span></span> |
| [<span data-ttu-id="4d212-191">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="4d212-191">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="4d212-192">Parametri della riga di comando</span><span class="sxs-lookup"><span data-stu-id="4d212-192">Command-line parameters</span></span> |
| [<span data-ttu-id="4d212-193">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="4d212-193">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="4d212-194">Origine personalizzata</span><span class="sxs-lookup"><span data-stu-id="4d212-194">Custom source</span></span> |
| [<span data-ttu-id="4d212-195">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="4d212-195">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="4d212-196">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="4d212-196">Environment variables</span></span> |
| [<span data-ttu-id="4d212-197">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="4d212-197">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="4d212-198">File (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="4d212-198">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="4d212-199">Provider di configurazione chiave-per-file</span><span class="sxs-lookup"><span data-stu-id="4d212-199">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="4d212-200">File della directory</span><span class="sxs-lookup"><span data-stu-id="4d212-200">Directory files</span></span> |
| [<span data-ttu-id="4d212-201">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="4d212-201">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="4d212-202">Raccolte in memoria</span><span class="sxs-lookup"><span data-stu-id="4d212-202">In-memory collections</span></span> |
| <span data-ttu-id="4d212-203">[Segreti utente (Secret Manager)](xref:security/app-secrets) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="4d212-203">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="4d212-204">File nella directory dei profili utente</span><span class="sxs-lookup"><span data-stu-id="4d212-204">File in the user profile directory</span></span> |

<span data-ttu-id="4d212-205">Le origini di configurazione vengono lette nell'ordine in cui vengono specificati i rispetti provider di configurazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="4d212-205">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="4d212-206">I provider di configurazione descritti in questo argomento vengono presentati in ordine alfabetico e non nell'ordine in cui potrebbe disporli il codice.</span><span class="sxs-lookup"><span data-stu-id="4d212-206">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="4d212-207">Ordinare i provider di configurazione nel codice in base alle specifiche priorità per le origini di configurazione sottostanti.</span><span class="sxs-lookup"><span data-stu-id="4d212-207">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="4d212-208">Una sequenza tipica di provider di configurazione è:</span><span class="sxs-lookup"><span data-stu-id="4d212-208">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="4d212-209">File (*appsettings.json*, *appsettings.{Environment}.json*, dove `{Environment}` è l'ambiente di hosting corrente dell'app)</span><span class="sxs-lookup"><span data-stu-id="4d212-209">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="4d212-210">Insieme di credenziali delle chiavi di Azure</span><span class="sxs-lookup"><span data-stu-id="4d212-210">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="4d212-211">[Segreti utente (Secret Manager)](xref:security/app-secrets) (sono nell'ambiente Development)</span><span class="sxs-lookup"><span data-stu-id="4d212-211">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="4d212-212">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="4d212-212">Environment variables</span></span>
1. <span data-ttu-id="4d212-213">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="4d212-213">Command-line arguments</span></span>

<span data-ttu-id="4d212-214">È pratica comune posizionare il provider di configurazione della riga di comando per ultimo in una serie di provider per consentire agli argomenti della riga di comando di sostituire la configurazione impostata da altri provider.</span><span class="sxs-lookup"><span data-stu-id="4d212-214">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="4d212-215">Questa sequenza di provider viene applicata quando si inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="4d212-215">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="4d212-216">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="4d212-216">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

## <a name="configureappconfiguration"></a><span data-ttu-id="4d212-217">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="4d212-217">ConfigureAppConfiguration</span></span>

<span data-ttu-id="4d212-218">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare i provider di configurazione dell'app, oltre a quelli aggiunti automaticamente da <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="4d212-218">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

<span data-ttu-id="4d212-219">La configurazione fornita all'app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> è disponibile durante l'avvio dell'app, incluso `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4d212-219">Configuration supplied to the app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="4d212-220">Per altre informazioni, vedere la sezione [Accedere alla configurazione durante l'avvio](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="4d212-220">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="4d212-221">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="4d212-221">Command-line Configuration Provider</span></span>

<span data-ttu-id="4d212-222"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> carica la configurazione da coppie chiave-valore di argomenti della riga di comando in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="4d212-222">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="4d212-223">Per attivare la configurazione della riga di comando, metodo di estensione <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> viene chiamato su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4d212-223">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4d212-224">`AddCommandLine` viene chiamato automaticamente quando si Inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="4d212-224">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="4d212-225">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="4d212-225">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="4d212-226">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="4d212-226">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="4d212-227">Una configurazione facoltativa da *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="4d212-227">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="4d212-228">[Segreti utente (Secret Manager)](xref:security/app-secrets) (nell'ambiente Development).</span><span class="sxs-lookup"><span data-stu-id="4d212-228">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="4d212-229">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="4d212-229">Environment variables.</span></span>

<span data-ttu-id="4d212-230">`CreateDefaultBuilder` aggiunge il provider di configurazione della riga di comando per ultimo.</span><span class="sxs-lookup"><span data-stu-id="4d212-230">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="4d212-231">Gli argomenti della riga di comando passati in fase di esecuzione sostituiscono la configurazione impostata dagli altri provider.</span><span class="sxs-lookup"><span data-stu-id="4d212-231">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="4d212-232">`CreateDefaultBuilder` opera durante la creazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="4d212-232">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="4d212-233">Pertanto, la configurazione della riga di comando attivata da `CreateDefaultBuilder` può influire sul modo in cui l'host viene configurato.</span><span class="sxs-lookup"><span data-stu-id="4d212-233">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="4d212-234">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="4d212-234">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="4d212-235">`AddCommandLine` è già stato chiamato da `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4d212-235">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="4d212-236">Se è necessario specificare la configurazione dell'app ed è ancora possibile eseguire l'override della configurazione con argomenti della riga di comando, chiamare i provider aggiuntivi dell'app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> e chiamare infine `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="4d212-236">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="4d212-237">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d212-237">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="4d212-238">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="4d212-238">**Example**</span></span>

<span data-ttu-id="4d212-239">L'app di esempio consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include una chiamata a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="4d212-239">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="4d212-240">Aprire un prompt dei comandi nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="4d212-240">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="4d212-241">Fornire un argomento della riga di comando per il comando `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="4d212-241">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="4d212-242">Dopo che l'app è in esecuzione, aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="4d212-242">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="4d212-243">Notare che l'output contiene la coppia chiave-valore per l'argomento della riga di comando di configurazione fornito per `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="4d212-243">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="4d212-244">Argomenti</span><span class="sxs-lookup"><span data-stu-id="4d212-244">Arguments</span></span>

<span data-ttu-id="4d212-245">Il valore deve seguire un segno di uguale (`=`) o la chiave deve avere un prefisso (`--` o `/`) quando il valore segue uno spazio.</span><span class="sxs-lookup"><span data-stu-id="4d212-245">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="4d212-246">Il valore può essere null se viene usato un segno di uguale (ad esempio, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="4d212-246">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="4d212-247">Prefisso chiave</span><span class="sxs-lookup"><span data-stu-id="4d212-247">Key prefix</span></span>               | <span data-ttu-id="4d212-248">Esempio</span><span class="sxs-lookup"><span data-stu-id="4d212-248">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="4d212-249">Nessun prefisso</span><span class="sxs-lookup"><span data-stu-id="4d212-249">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="4d212-250">Due trattini (`--`)</span><span class="sxs-lookup"><span data-stu-id="4d212-250">Two dashes (`--`)</span></span>        | <span data-ttu-id="4d212-251">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="4d212-251">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="4d212-252">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="4d212-252">Forward slash (`/`)</span></span>      | <span data-ttu-id="4d212-253">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="4d212-253">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="4d212-254">Nello stesso comando, non mischiare coppie chiave-valore di argomenti della riga di comando che usano un segno di uguale con coppie chiave-valore che usano uno spazio.</span><span class="sxs-lookup"><span data-stu-id="4d212-254">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="4d212-255">Comandi di esempio:</span><span class="sxs-lookup"><span data-stu-id="4d212-255">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="4d212-256">Mapping di sostituzione</span><span class="sxs-lookup"><span data-stu-id="4d212-256">Switch mappings</span></span>

<span data-ttu-id="4d212-257">I mapping di sostituzione consentono di usare la logica di sostituzione del nome della chiave.</span><span class="sxs-lookup"><span data-stu-id="4d212-257">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="4d212-258">Quando si crea manualmente la configurazione con un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, è possibile specificare un dizionario di mapping di sostituzione per il metodo <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="4d212-258">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="4d212-259">Quando viene utilizzato il dizionario dei mapping di sostituzione, nel dizionario viene controllata la presenza di una chiave corrispondente alla chiave fornita da un argomento della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="4d212-259">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="4d212-260">Se la chiave della riga di comando viene trovata nel dizionario, il valore del dizionario (la sostituzione della chiave) viene passato nuovamente per impostare la coppia chiave-valore nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="4d212-260">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="4d212-261">Un mapping di sostituzione è necessario per le chiavi della riga di comando con un trattino singolo (`-`) come prefisso.</span><span class="sxs-lookup"><span data-stu-id="4d212-261">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="4d212-262">Regole principali del dizionario dei mapping di sostituzione:</span><span class="sxs-lookup"><span data-stu-id="4d212-262">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="4d212-263">Le sostituzioni devono iniziare con un trattino (`-`) o un trattino doppio (`--`).</span><span class="sxs-lookup"><span data-stu-id="4d212-263">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="4d212-264">Il dizionario dei mapping di sostituzione non deve contenere chiavi duplicate.</span><span class="sxs-lookup"><span data-stu-id="4d212-264">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="4d212-265">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="4d212-265">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="4d212-266">Come illustrato nell'esempio precedente, la chiamata a `CreateDefaultBuilder` non deve passare argomenti quando vengono usati i mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="4d212-266">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="4d212-267">La chiamata `AddCommandLine` del metodo `CreateDefaultBuilder` non include sostituzioni mappate e non è possibile passare il dizionario dei mapping di sostituzione a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4d212-267">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="4d212-268">Se gli argomenti includono una sostituzione mappata e vengono passati a `CreateDefaultBuilder`, l'inizializzazione del rispettivo provider `AddCommandLine` non riesce con <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="4d212-268">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="4d212-269">La soluzione consiste nel non passare gli argomenti a `CreateDefaultBuilder` consentendo invece al metodo `AddCommandLine` del metodo `ConfigurationBuilder` di elaborare entrambi gli argomenti e il dizionario dei mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="4d212-269">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="4d212-270">Il dizionario dei mapping di sostituzione creato contiene i dati visualizzati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="4d212-270">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="4d212-271">Chiave</span><span class="sxs-lookup"><span data-stu-id="4d212-271">Key</span></span>       | <span data-ttu-id="4d212-272">Value</span><span class="sxs-lookup"><span data-stu-id="4d212-272">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="4d212-273">Se le chiavi con mapping di sostituzione vengono usate all'avvio dell'app, la configurazione riceve il valore di configurazione per la chiave fornita dal dizionario:</span><span class="sxs-lookup"><span data-stu-id="4d212-273">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="4d212-274">Dopo aver eseguito il comando precedente, la configurazione contiene i valori mostrati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="4d212-274">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="4d212-275">Chiave</span><span class="sxs-lookup"><span data-stu-id="4d212-275">Key</span></span>               | <span data-ttu-id="4d212-276">Value</span><span class="sxs-lookup"><span data-stu-id="4d212-276">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="4d212-277">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="4d212-277">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="4d212-278"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> carica la configurazione da coppie chiave-valore di variabili di ambiente in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="4d212-278">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="4d212-279">Per attivare la configurazione delle variabili di ambiente, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4d212-279">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4d212-280">Quando si utilizzano chiavi gerarchiche in variabili di ambiente, il separatore due punti (`:`) potrebbe non funzionare in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="4d212-280">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="4d212-281">Il doppio carattere di sottolineatura (`__`) è supportato da tutte le piattaforme e viene sostituito con due punti.</span><span class="sxs-lookup"><span data-stu-id="4d212-281">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="4d212-282">[Servizio App di Azure](https://azure.microsoft.com/services/app-service/) consente di impostare variabili di ambiente nel portale di Azure che possono sostituire la configurazione delle app tramite il provider di configurazione delle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="4d212-282">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="4d212-283">Per altre informazioni, vedere [App Azure: Eseguire l'override della configurazione delle app usando il portale di Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="4d212-283">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="4d212-284">`AddEnvironmentVariables` viene chiamato automaticamente per le variabili di ambiente con prefisso `ASPNETCORE_` quando si inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4d212-284">`AddEnvironmentVariables` is automatically called for environment variables prefixed with `ASPNETCORE_` when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="4d212-285">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="4d212-285">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="4d212-286">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="4d212-286">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="4d212-287">Configurazione delle app dalle variabili di ambiente senza prefisso chiamando `AddEnvironmentVariables` senza prefisso.</span><span class="sxs-lookup"><span data-stu-id="4d212-287">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="4d212-288">Una configurazione facoltativa da *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="4d212-288">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="4d212-289">[Segreti utente (Secret Manager)](xref:security/app-secrets) (nell'ambiente Development).</span><span class="sxs-lookup"><span data-stu-id="4d212-289">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="4d212-290">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="4d212-290">Command-line arguments.</span></span>

<span data-ttu-id="4d212-291">Il provider di configurazione delle variabili di ambiente viene chiamato dopo aver stabilito la configurazione dai segreti utente e dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="4d212-291">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="4d212-292">La chiamata del provider in questa posizione consente alle variabili di ambiente lette in fase di esecuzione di sostituire la configurazione impostata dai segreti utente e dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="4d212-292">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="4d212-293">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="4d212-293">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="4d212-294">Se è necessario specificare la configurazione dell'app da variabili di ambiente aggiuntive, chiamare i provider aggiuntivi dell'app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> e chiamare `AddEnvironmentVariables` con il prefisso.</span><span class="sxs-lookup"><span data-stu-id="4d212-294">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="4d212-295">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d212-295">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="4d212-296">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="4d212-296">**Example**</span></span>

<span data-ttu-id="4d212-297">L'app di esempio consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include una chiamata a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="4d212-297">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="4d212-298">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="4d212-298">Run the sample app.</span></span> <span data-ttu-id="4d212-299">Aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="4d212-299">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="4d212-300">Notare che l'output contiene la coppia chiave-valore per la variabile di ambiente `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="4d212-300">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="4d212-301">Il valore riflette l'ambiente in cui l'app è in esecuzione, in genere `Development` durante l'esecuzione locale.</span><span class="sxs-lookup"><span data-stu-id="4d212-301">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="4d212-302">Per mantenere breve l'elenco delle variabili di ambiente restituito dall'app, l'app filtra le variabili di ambiente includendo quelle che iniziano come segue:</span><span class="sxs-lookup"><span data-stu-id="4d212-302">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="4d212-303">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="4d212-303">ASPNETCORE_</span></span>
* <span data-ttu-id="4d212-304">urls</span><span class="sxs-lookup"><span data-stu-id="4d212-304">urls</span></span>
* <span data-ttu-id="4d212-305">Registrazione</span><span class="sxs-lookup"><span data-stu-id="4d212-305">Logging</span></span>
* <span data-ttu-id="4d212-306">ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="4d212-306">ENVIRONMENT</span></span>
* <span data-ttu-id="4d212-307">contentRoot</span><span class="sxs-lookup"><span data-stu-id="4d212-307">contentRoot</span></span>
* <span data-ttu-id="4d212-308">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="4d212-308">AllowedHosts</span></span>
* <span data-ttu-id="4d212-309">applicationName</span><span class="sxs-lookup"><span data-stu-id="4d212-309">applicationName</span></span>
* <span data-ttu-id="4d212-310">CommandLine</span><span class="sxs-lookup"><span data-stu-id="4d212-310">CommandLine</span></span>

<span data-ttu-id="4d212-311">Se si desidera esporre tutte le variabili di ambiente disponibili all'app, modificare `FilteredConfiguration` in *Pages/Index.cshtml.cs* come segue:</span><span class="sxs-lookup"><span data-stu-id="4d212-311">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="4d212-312">Prefissi</span><span class="sxs-lookup"><span data-stu-id="4d212-312">Prefixes</span></span>

<span data-ttu-id="4d212-313">Le variabili di ambiente caricate nella configurazione dell'app vengono filtrate quando si specifica un prefisso per il metodo `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="4d212-313">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="4d212-314">Ad esempio, per filtrare le variabili di ambiente in base al prefisso `CUSTOM_`, fornire il prefisso al provider di configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d212-314">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="4d212-315">Il prefisso viene rimosso quando vengono create le coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d212-315">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="4d212-316">Il metodo di servizio statico `CreateDefaultBuilder` crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> per stabilire l'host dell'app.</span><span class="sxs-lookup"><span data-stu-id="4d212-316">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="4d212-317">Quando viene creato `WebHostBuilder`, la configurazione dell'host corrispondente viene individuata nelle variabili di ambiente con il prefisso `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="4d212-317">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

<span data-ttu-id="4d212-318">**Prefissi della stringa di connessione**</span><span class="sxs-lookup"><span data-stu-id="4d212-318">**Connection string prefixes**</span></span>

<span data-ttu-id="4d212-319">L'API di configurazione prevede regole di elaborazione speciali per quattro variabili di ambiente di stringa di connessione coinvolte nella configurazione di stringhe di connessione di Azure per l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="4d212-319">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="4d212-320">Le variabili di ambiente con i prefissi indicati nella tabella vengono caricate nell'app se non viene fornito alcun prefisso a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="4d212-320">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="4d212-321">Prefisso della stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="4d212-321">Connection string prefix</span></span> | <span data-ttu-id="4d212-322">Provider</span><span class="sxs-lookup"><span data-stu-id="4d212-322">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="4d212-323">Provider personalizzato</span><span class="sxs-lookup"><span data-stu-id="4d212-323">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="4d212-324">MySQL</span><span class="sxs-lookup"><span data-stu-id="4d212-324">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="4d212-325">Database SQL di Azure</span><span class="sxs-lookup"><span data-stu-id="4d212-325">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="4d212-326">SQL Server</span><span class="sxs-lookup"><span data-stu-id="4d212-326">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="4d212-327">Quando una variabile di ambiente viene individuata e caricata nella configurazione con uno qualsiasi dei quattro prefissi indicati nella tabella:</span><span class="sxs-lookup"><span data-stu-id="4d212-327">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="4d212-328">La chiave di configurazione viene creata rimuovendo il prefisso della variabile di ambiente e aggiungendo una sezione per la chiave di configurazione (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="4d212-328">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="4d212-329">Viene creata una nuova coppia chiave-valore della configurazione che rappresenta il provider di connessione al database (ad eccezione di `CUSTOMCONNSTR_`, che non ha un provider dichiarato).</span><span class="sxs-lookup"><span data-stu-id="4d212-329">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="4d212-330">Chiave della variabile di ambiente</span><span class="sxs-lookup"><span data-stu-id="4d212-330">Environment variable key</span></span> | <span data-ttu-id="4d212-331">Chiave di configurazione convertita</span><span class="sxs-lookup"><span data-stu-id="4d212-331">Converted configuration key</span></span> | <span data-ttu-id="4d212-332">Voce di configurazione del provider</span><span class="sxs-lookup"><span data-stu-id="4d212-332">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="4d212-333">Voce di configurazione non creata.</span><span class="sxs-lookup"><span data-stu-id="4d212-333">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="4d212-334">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="4d212-334">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="4d212-335">Valore: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="4d212-335">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="4d212-336">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="4d212-336">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="4d212-337">Valore: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="4d212-337">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="4d212-338">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="4d212-338">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="4d212-339">Valore: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="4d212-339">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="4d212-340">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="4d212-340">File Configuration Provider</span></span>

<span data-ttu-id="4d212-341"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> è la classe base per il caricamento della configurazione dal file system.</span><span class="sxs-lookup"><span data-stu-id="4d212-341"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="4d212-342">I provider di configurazione seguenti sono dedicati per specifici tipi di file:</span><span class="sxs-lookup"><span data-stu-id="4d212-342">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="4d212-343">Provider di configurazione INI</span><span class="sxs-lookup"><span data-stu-id="4d212-343">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="4d212-344">Provider di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="4d212-344">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="4d212-345">Provider di configurazione XML</span><span class="sxs-lookup"><span data-stu-id="4d212-345">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="4d212-346">Provider di configurazione INI</span><span class="sxs-lookup"><span data-stu-id="4d212-346">INI Configuration Provider</span></span>

<span data-ttu-id="4d212-347"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> carica la configurazione da coppie chiave-valore di file INI in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="4d212-347">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="4d212-348">Per attivare la configurazione di file INI, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4d212-348">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4d212-349">I due punti possono essere usati come delimitatore di sezione nella configurazione di file INI.</span><span class="sxs-lookup"><span data-stu-id="4d212-349">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="4d212-350">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="4d212-350">Overloads permit specifying:</span></span>

* <span data-ttu-id="4d212-351">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="4d212-351">Whether the file is optional.</span></span>
* <span data-ttu-id="4d212-352">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="4d212-352">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="4d212-353">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="4d212-353">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="4d212-354">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="4d212-354">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="4d212-355">Il percorso di base è impostato con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="4d212-355">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="4d212-356">`SetBasePath` è incluso nel pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4d212-356">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4d212-357">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d212-357">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="4d212-358">Il percorso di base è impostato con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="4d212-358">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="4d212-359">`SetBasePath` è incluso nel pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4d212-359">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4d212-360">Un esempio generico di un file di configurazione INI:</span><span class="sxs-lookup"><span data-stu-id="4d212-360">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="4d212-361">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="4d212-361">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="4d212-362">section0:key0</span><span class="sxs-lookup"><span data-stu-id="4d212-362">section0:key0</span></span>
* <span data-ttu-id="4d212-363">section0:key1</span><span class="sxs-lookup"><span data-stu-id="4d212-363">section0:key1</span></span>
* <span data-ttu-id="4d212-364">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="4d212-364">section1:subsection:key</span></span>
* <span data-ttu-id="4d212-365">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="4d212-365">section2:subsection0:key</span></span>
* <span data-ttu-id="4d212-366">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="4d212-366">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="4d212-367">Provider di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="4d212-367">JSON Configuration Provider</span></span>

<span data-ttu-id="4d212-368">Il <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> carica la configurazione da coppie chiave-valore di file JSON in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="4d212-368">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="4d212-369">Per attivare la configurazione di file JSON, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4d212-369">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4d212-370">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="4d212-370">Overloads permit specifying:</span></span>

* <span data-ttu-id="4d212-371">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="4d212-371">Whether the file is optional.</span></span>
* <span data-ttu-id="4d212-372">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="4d212-372">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="4d212-373">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="4d212-373">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="4d212-374">`AddJsonFile` viene chiamato automaticamente due volte quando si inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="4d212-374">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="4d212-375">Il metodo viene chiamato per caricare la configurazione da:</span><span class="sxs-lookup"><span data-stu-id="4d212-375">The method is called to load configuration from:</span></span>

* <span data-ttu-id="4d212-376">*appsettings.json* &ndash; Questo file viene letto per primo.</span><span class="sxs-lookup"><span data-stu-id="4d212-376">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="4d212-377">La versione dell'ambiente del file può sostituire i valori forniti dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="4d212-377">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="4d212-378">*appsettings.{Environment}.json* &ndash; La versione dell'ambiente del file viene caricata in base a [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="4d212-378">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="4d212-379">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="4d212-379">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="4d212-380">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="4d212-380">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="4d212-381">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="4d212-381">Environment variables.</span></span>
* <span data-ttu-id="4d212-382">[Segreti utente (Secret Manager)](xref:security/app-secrets) (nell'ambiente Development).</span><span class="sxs-lookup"><span data-stu-id="4d212-382">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="4d212-383">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="4d212-383">Command-line arguments.</span></span>

<span data-ttu-id="4d212-384">Il provider di configurazione JSON viene stabilito per primo.</span><span class="sxs-lookup"><span data-stu-id="4d212-384">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="4d212-385">I segreti utente, le variabili di ambiente e gli argomenti della riga di comando sostituiscono quindi la configurazione impostata dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="4d212-385">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="4d212-386">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app per file diversi da *appsettings.json* e *appsettings.{Environment}.json*:</span><span class="sxs-lookup"><span data-stu-id="4d212-386">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

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

<span data-ttu-id="4d212-387">Il percorso di base è impostato con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="4d212-387">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="4d212-388">`SetBasePath` è incluso nel pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4d212-388">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4d212-389">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d212-389">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="4d212-390">Il percorso di base è impostato con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="4d212-390">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="4d212-391">`SetBasePath` è incluso nel pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4d212-391">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4d212-392">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="4d212-392">**Example**</span></span>

<span data-ttu-id="4d212-393">L'app di esempio consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include due chiamate a `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="4d212-393">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="4d212-394">La configurazione viene caricata da *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="4d212-394">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

1. <span data-ttu-id="4d212-395">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="4d212-395">Run the sample app.</span></span> <span data-ttu-id="4d212-396">Aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="4d212-396">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="4d212-397">Notare che l'output contiene coppie chiave-valore per la configurazione mostrata nella tabella in base all'ambiente.</span><span class="sxs-lookup"><span data-stu-id="4d212-397">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="4d212-398">Le chiavi di configurazione di registrazione usano i due punti (`:`) come separatore gerarchico.</span><span class="sxs-lookup"><span data-stu-id="4d212-398">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="4d212-399">Chiave</span><span class="sxs-lookup"><span data-stu-id="4d212-399">Key</span></span>                        | <span data-ttu-id="4d212-400">Valore Development</span><span class="sxs-lookup"><span data-stu-id="4d212-400">Development Value</span></span> | <span data-ttu-id="4d212-401">Valore Production</span><span class="sxs-lookup"><span data-stu-id="4d212-401">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="4d212-402">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="4d212-402">Logging:LogLevel:System</span></span>    | <span data-ttu-id="4d212-403">Informazioni</span><span class="sxs-lookup"><span data-stu-id="4d212-403">Information</span></span>       | <span data-ttu-id="4d212-404">Informazioni</span><span class="sxs-lookup"><span data-stu-id="4d212-404">Information</span></span>      |
| <span data-ttu-id="4d212-405">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="4d212-405">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="4d212-406">Informazioni</span><span class="sxs-lookup"><span data-stu-id="4d212-406">Information</span></span>       | <span data-ttu-id="4d212-407">Informazioni</span><span class="sxs-lookup"><span data-stu-id="4d212-407">Information</span></span>      |
| <span data-ttu-id="4d212-408">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="4d212-408">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="4d212-409">Debug</span><span class="sxs-lookup"><span data-stu-id="4d212-409">Debug</span></span>             | <span data-ttu-id="4d212-410">Error</span><span class="sxs-lookup"><span data-stu-id="4d212-410">Error</span></span>            |
| <span data-ttu-id="4d212-411">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="4d212-411">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="4d212-412">Provider di configurazione XML</span><span class="sxs-lookup"><span data-stu-id="4d212-412">XML Configuration Provider</span></span>

<span data-ttu-id="4d212-413">Il <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> carica la configurazione da coppie chiave-valore di file XML in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="4d212-413">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="4d212-414">Per attivare la configurazione di file XML, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4d212-414">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4d212-415">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="4d212-415">Overloads permit specifying:</span></span>

* <span data-ttu-id="4d212-416">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="4d212-416">Whether the file is optional.</span></span>
* <span data-ttu-id="4d212-417">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="4d212-417">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="4d212-418">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="4d212-418">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="4d212-419">Il nodo radice del file di configurazione viene ignorato quando vengono create le coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d212-419">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="4d212-420">Non specificare una definizione DTD (Document Type Definition) o uno spazio dei nomi nel file.</span><span class="sxs-lookup"><span data-stu-id="4d212-420">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="4d212-421">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="4d212-421">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="4d212-422">Il percorso di base è impostato con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="4d212-422">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="4d212-423">`SetBasePath` è incluso nel pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4d212-423">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4d212-424">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d212-424">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="4d212-425">Il percorso di base è impostato con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="4d212-425">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="4d212-426">`SetBasePath` è incluso nel pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4d212-426">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4d212-427">I file di configurazione XML possono usare nomi di elementi distinti per le sezioni ripetute:</span><span class="sxs-lookup"><span data-stu-id="4d212-427">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="4d212-428">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="4d212-428">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="4d212-429">section0:key0</span><span class="sxs-lookup"><span data-stu-id="4d212-429">section0:key0</span></span>
* <span data-ttu-id="4d212-430">section0:key1</span><span class="sxs-lookup"><span data-stu-id="4d212-430">section0:key1</span></span>
* <span data-ttu-id="4d212-431">section1:key0</span><span class="sxs-lookup"><span data-stu-id="4d212-431">section1:key0</span></span>
* <span data-ttu-id="4d212-432">section1:key1</span><span class="sxs-lookup"><span data-stu-id="4d212-432">section1:key1</span></span>

<span data-ttu-id="4d212-433">La ripetizione di elementi che usano lo stesso nome di elemento funziona se si usa l'attributo `name` per distinguere gli elementi:</span><span class="sxs-lookup"><span data-stu-id="4d212-433">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="4d212-434">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="4d212-434">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="4d212-435">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="4d212-435">section:section0:key:key0</span></span>
* <span data-ttu-id="4d212-436">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="4d212-436">section:section0:key:key1</span></span>
* <span data-ttu-id="4d212-437">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="4d212-437">section:section1:key:key0</span></span>
* <span data-ttu-id="4d212-438">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="4d212-438">section:section1:key:key1</span></span>

<span data-ttu-id="4d212-439">È possibile usare attributi per fornire valori:</span><span class="sxs-lookup"><span data-stu-id="4d212-439">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="4d212-440">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="4d212-440">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="4d212-441">key:attribute</span><span class="sxs-lookup"><span data-stu-id="4d212-441">key:attribute</span></span>
* <span data-ttu-id="4d212-442">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="4d212-442">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="4d212-443">Provider di configurazione KeyPerFile</span><span class="sxs-lookup"><span data-stu-id="4d212-443">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="4d212-444">Il <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> usa i file di una directory come coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d212-444">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="4d212-445">La chiave è il nome del file.</span><span class="sxs-lookup"><span data-stu-id="4d212-445">The key is the file name.</span></span> <span data-ttu-id="4d212-446">Il valore contiene il contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="4d212-446">The value contains the file's contents.</span></span> <span data-ttu-id="4d212-447">Il provider di configurazione KeyPerFile viene usato in scenari di hosting Docker.</span><span class="sxs-lookup"><span data-stu-id="4d212-447">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="4d212-448">Per attivare la configurazione chiave-per-file, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4d212-448">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="4d212-449">Il `directoryPath` per i file deve essere un percorso assoluto.</span><span class="sxs-lookup"><span data-stu-id="4d212-449">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="4d212-450">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="4d212-450">Overloads permit specifying:</span></span>

* <span data-ttu-id="4d212-451">Un delegato `Action<KeyPerFileConfigurationSource>` che configura l'origine.</span><span class="sxs-lookup"><span data-stu-id="4d212-451">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="4d212-452">Se la directory è facoltativa e il percorso della directory.</span><span class="sxs-lookup"><span data-stu-id="4d212-452">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="4d212-453">Il doppio carattere di sottolineatura (`__`) viene usato come delimitatore per le chiavi di configurazione nei nomi dei file.</span><span class="sxs-lookup"><span data-stu-id="4d212-453">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="4d212-454">Ad esempio, il nome di file `Logging__LogLevel__System` produce la chiave di configurazione `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="4d212-454">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="4d212-455">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="4d212-455">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="4d212-456">Il percorso di base è impostato con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="4d212-456">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="4d212-457">`SetBasePath` è incluso nel pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4d212-457">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4d212-458">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d212-458">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="4d212-459">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="4d212-459">Memory Configuration Provider</span></span>

<span data-ttu-id="4d212-460">Il <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> usa una raccolta in memoria come coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d212-460">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="4d212-461">Per attivare la configurazione della raccolta in memoria, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4d212-461">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4d212-462">Il provider di configurazione può essere inizializzato con un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="4d212-462">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="4d212-463">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="4d212-463">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="4d212-464">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d212-464">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="4d212-465">GetValue</span><span class="sxs-lookup"><span data-stu-id="4d212-465">GetValue</span></span>

<span data-ttu-id="4d212-466">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) estrae un valore dalla configurazione con una chiave specificata e lo converte nel tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="4d212-466">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="4d212-467">Un overload consente di specificare un valore predefinito se non viene trovata la chiave.</span><span class="sxs-lookup"><span data-stu-id="4d212-467">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="4d212-468">L'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="4d212-468">The following example:</span></span>

* <span data-ttu-id="4d212-469">Estrae il valore di stringa dalla configurazione con la chiave `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="4d212-469">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="4d212-470">Se `NumberKey` non viene trovato nelle chiavi di configurazione, viene usato il valore predefinito di `99`.</span><span class="sxs-lookup"><span data-stu-id="4d212-470">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="4d212-471">Assegna al valore il tipo `int`.</span><span class="sxs-lookup"><span data-stu-id="4d212-471">Types the value as an `int`.</span></span>
* <span data-ttu-id="4d212-472">Memorizza il valore nella proprietà `NumberConfig` per l'uso da parte della pagina.</span><span class="sxs-lookup"><span data-stu-id="4d212-472">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="4d212-473">GetSection, GetChildren ed Exists</span><span class="sxs-lookup"><span data-stu-id="4d212-473">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="4d212-474">Per gli esempi che seguono, prendere in considerazione il file JSON seguente.</span><span class="sxs-lookup"><span data-stu-id="4d212-474">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="4d212-475">Vengono trovate quattro chiavi in due sezioni, una delle quali include una coppia di sottosezioni:</span><span class="sxs-lookup"><span data-stu-id="4d212-475">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="4d212-476">Quando il file viene letto nella configurazione, vengono create le chiavi gerarchiche univoche seguenti per contenere i valori di configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d212-476">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="4d212-477">section0:key0</span><span class="sxs-lookup"><span data-stu-id="4d212-477">section0:key0</span></span>
* <span data-ttu-id="4d212-478">section0:key1</span><span class="sxs-lookup"><span data-stu-id="4d212-478">section0:key1</span></span>
* <span data-ttu-id="4d212-479">section1:key0</span><span class="sxs-lookup"><span data-stu-id="4d212-479">section1:key0</span></span>
* <span data-ttu-id="4d212-480">section1:key1</span><span class="sxs-lookup"><span data-stu-id="4d212-480">section1:key1</span></span>
* <span data-ttu-id="4d212-481">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="4d212-481">section2:subsection0:key0</span></span>
* <span data-ttu-id="4d212-482">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="4d212-482">section2:subsection0:key1</span></span>
* <span data-ttu-id="4d212-483">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="4d212-483">section2:subsection1:key0</span></span>
* <span data-ttu-id="4d212-484">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="4d212-484">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="4d212-485">GetSection</span><span class="sxs-lookup"><span data-stu-id="4d212-485">GetSection</span></span>

<span data-ttu-id="4d212-486">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) estrae una sottosezione della configurazione con la chiave di sottosezione specificata.</span><span class="sxs-lookup"><span data-stu-id="4d212-486">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span> <span data-ttu-id="4d212-487">`GetSection` è incluso nel pacchetto [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4d212-487">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4d212-488">Per restituire una <xref:Microsoft.Extensions.Configuration.IConfigurationSection> che contiene solo le coppie chiave-valore in `section1`, chiamare `GetSection` e fornire il nome della sezione:</span><span class="sxs-lookup"><span data-stu-id="4d212-488">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="4d212-489">`configSection` non ha un valore, ma solo una chiave e un percorso.</span><span class="sxs-lookup"><span data-stu-id="4d212-489">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="4d212-490">In modo analogo, per ottenere i valori per le chiavi in `section2:subsection0`, chiamare `GetSection` e fornire il percorso della sezione:</span><span class="sxs-lookup"><span data-stu-id="4d212-490">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="4d212-491">`GetSection` non restituisce mai `null`.</span><span class="sxs-lookup"><span data-stu-id="4d212-491">`GetSection` never returns `null`.</span></span> <span data-ttu-id="4d212-492">Se non viene trovata una sezione corrispondente, viene restituita una `IConfigurationSection` vuota.</span><span class="sxs-lookup"><span data-stu-id="4d212-492">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="4d212-493">Quando `GetSection` restituisce una sezione corrispondente, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> non viene compilata.</span><span class="sxs-lookup"><span data-stu-id="4d212-493">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="4d212-494">Quando la sezione esiste, vengono restituiti <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> e <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path>.</span><span class="sxs-lookup"><span data-stu-id="4d212-494">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="4d212-495">GetChildren</span><span class="sxs-lookup"><span data-stu-id="4d212-495">GetChildren</span></span>

<span data-ttu-id="4d212-496">Una chiamata a [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) per `section2` ottiene una `IEnumerable<IConfigurationSection>` che include:</span><span class="sxs-lookup"><span data-stu-id="4d212-496">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="4d212-497">Esistente</span><span class="sxs-lookup"><span data-stu-id="4d212-497">Exists</span></span>

<span data-ttu-id="4d212-498">Usare [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) per determinare se esiste una sezione di configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d212-498">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="4d212-499">Considerati i dati di esempio, `sectionExists` è `false` perché non esiste una sezione `section2:subsection2` nei dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d212-499">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="4d212-500">Eseguire l'associazione a una classe</span><span class="sxs-lookup"><span data-stu-id="4d212-500">Bind to a class</span></span>

<span data-ttu-id="4d212-501">La configurazione può essere associata alle classi che rappresentano gruppi di impostazioni correlate usando il *modello di opzioni*.</span><span class="sxs-lookup"><span data-stu-id="4d212-501">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="4d212-502">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="4d212-502">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="4d212-503">I valori di configurazione vengono restituiti come stringhe, ma la chiamata di <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> consente la costruzione di oggetti [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="4d212-503">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="4d212-504">`Bind` è incluso nel pacchetto [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4d212-504">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4d212-505">L'app di esempio contiene un modello `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="4d212-505">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="4d212-506">La sezione `starship` del file *starship.json* crea la configurazione quando l'app di esempio usa il provider di configurazione JSON per caricare la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d212-506">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

<span data-ttu-id="4d212-507">Vengono create le coppie chiave-valore della configurazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="4d212-507">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="4d212-508">Chiave</span><span class="sxs-lookup"><span data-stu-id="4d212-508">Key</span></span>                   | <span data-ttu-id="4d212-509">Value</span><span class="sxs-lookup"><span data-stu-id="4d212-509">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="4d212-510">starship:name</span><span class="sxs-lookup"><span data-stu-id="4d212-510">starship:name</span></span>         | <span data-ttu-id="4d212-511">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="4d212-511">USS Enterprise</span></span>                                    |
| <span data-ttu-id="4d212-512">starship:registry</span><span class="sxs-lookup"><span data-stu-id="4d212-512">starship:registry</span></span>     | <span data-ttu-id="4d212-513">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="4d212-513">NCC-1701</span></span>                                          |
| <span data-ttu-id="4d212-514">starship:class</span><span class="sxs-lookup"><span data-stu-id="4d212-514">starship:class</span></span>        | <span data-ttu-id="4d212-515">Constitution</span><span class="sxs-lookup"><span data-stu-id="4d212-515">Constitution</span></span>                                      |
| <span data-ttu-id="4d212-516">starship:length</span><span class="sxs-lookup"><span data-stu-id="4d212-516">starship:length</span></span>       | <span data-ttu-id="4d212-517">304.8</span><span class="sxs-lookup"><span data-stu-id="4d212-517">304.8</span></span>                                             |
| <span data-ttu-id="4d212-518">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="4d212-518">starship:commissioned</span></span> | <span data-ttu-id="4d212-519">False</span><span class="sxs-lookup"><span data-stu-id="4d212-519">False</span></span>                                             |
| <span data-ttu-id="4d212-520">trademark</span><span class="sxs-lookup"><span data-stu-id="4d212-520">trademark</span></span>             | <span data-ttu-id="4d212-521">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="4d212-521">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="4d212-522">L'app di esempio chiama `GetSection` con la chiave `starship`.</span><span class="sxs-lookup"><span data-stu-id="4d212-522">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="4d212-523">Le coppie chiave-valore `starship` sono isolate.</span><span class="sxs-lookup"><span data-stu-id="4d212-523">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="4d212-524">Il metodo `Bind` viene chiamato sulla sottosezione passando un'istanza della classe `Starship`.</span><span class="sxs-lookup"><span data-stu-id="4d212-524">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="4d212-525">Dopo aver associato i valori dell'istanza, l'istanza viene assegnata a una proprietà per il rendering:</span><span class="sxs-lookup"><span data-stu-id="4d212-525">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

<span data-ttu-id="4d212-526">`GetSection` è incluso nel pacchetto [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4d212-526">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="4d212-527">Associazione a un oggetto grafico</span><span class="sxs-lookup"><span data-stu-id="4d212-527">Bind to an object graph</span></span>

<span data-ttu-id="4d212-528"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> è in grado di associare un intero oggetto grafico POCO.</span><span class="sxs-lookup"><span data-stu-id="4d212-528"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="4d212-529">`Bind` è incluso nel pacchetto [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4d212-529">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4d212-530">L'esempio contiene un modello `TvShow` il cui oggetto grafico include `Metadata` e le classi `Actors` (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="4d212-530">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="4d212-531">L'app di esempio ha un file *tvshow.xml* che contiene i dati di configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d212-531">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="4d212-532">La configurazione viene associata all'intero oggetto grafico `TvShow` con il metodo `Bind`.</span><span class="sxs-lookup"><span data-stu-id="4d212-532">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="4d212-533">L'istanza associata viene assegnata a una proprietà per il rendering:</span><span class="sxs-lookup"><span data-stu-id="4d212-533">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="4d212-534">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) associa e restituisce il tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="4d212-534">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="4d212-535">`Get<T>` può essere più comodo che usare `Bind`.</span><span class="sxs-lookup"><span data-stu-id="4d212-535">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="4d212-536">Il codice seguente mostra come usare `Get<T>` con l'esempio precedente, che consente di assegnare l'istanza associata direttamente alla proprietà usata per il rendering:</span><span class="sxs-lookup"><span data-stu-id="4d212-536">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

<span data-ttu-id="4d212-537"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> è incluso nel pacchetto [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4d212-537"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="4d212-538">`Get<T>` è disponibile in ASP.NET Core 1.1 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="4d212-538">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="4d212-539">`GetSection` è incluso nel pacchetto [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4d212-539">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="4d212-540">Associare una matrice a una classe</span><span class="sxs-lookup"><span data-stu-id="4d212-540">Bind an array to a class</span></span>

<span data-ttu-id="4d212-541">*L'app di esempio dimostra i concetti spiegati in questa sezione.*</span><span class="sxs-lookup"><span data-stu-id="4d212-541">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="4d212-542">Il <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supporta l'associazione di matrici agli oggetti usando gli indici delle matrici nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d212-542">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="4d212-543">Qualsiasi formato di matrice che espone un segmento chiave numerico (`:0:`, `:1:`, &hellip; `:{n}:`) supporta l'associazione di matrici a una matrice di classi POCO.</span><span class="sxs-lookup"><span data-stu-id="4d212-543">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span> <span data-ttu-id="4d212-544">"Bind" è incluso nel pacchetto [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4d212-544">\`Bind\`\` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

> [!NOTE]
> <span data-ttu-id="4d212-545">L'associazione viene fornita per convenzione.</span><span class="sxs-lookup"><span data-stu-id="4d212-545">Binding is provided by convention.</span></span> <span data-ttu-id="4d212-546">I provider di configurazione personalizzati non devono implementare l'associazione di matrici.</span><span class="sxs-lookup"><span data-stu-id="4d212-546">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="4d212-547">**Elaborazione di matrici in memoria**</span><span class="sxs-lookup"><span data-stu-id="4d212-547">**In-memory array processing**</span></span>

<span data-ttu-id="4d212-548">Prendere in considerazione le chiavi di configurazione e i valori indicati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="4d212-548">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="4d212-549">Chiave</span><span class="sxs-lookup"><span data-stu-id="4d212-549">Key</span></span>             | <span data-ttu-id="4d212-550">Valore</span><span class="sxs-lookup"><span data-stu-id="4d212-550">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="4d212-551">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="4d212-551">array:entries:0</span></span> | <span data-ttu-id="4d212-552">value0</span><span class="sxs-lookup"><span data-stu-id="4d212-552">value0</span></span> |
| <span data-ttu-id="4d212-553">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="4d212-553">array:entries:1</span></span> | <span data-ttu-id="4d212-554">value1</span><span class="sxs-lookup"><span data-stu-id="4d212-554">value1</span></span> |
| <span data-ttu-id="4d212-555">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="4d212-555">array:entries:2</span></span> | <span data-ttu-id="4d212-556">value2</span><span class="sxs-lookup"><span data-stu-id="4d212-556">value2</span></span> |
| <span data-ttu-id="4d212-557">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="4d212-557">array:entries:4</span></span> | <span data-ttu-id="4d212-558">value4</span><span class="sxs-lookup"><span data-stu-id="4d212-558">value4</span></span> |
| <span data-ttu-id="4d212-559">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="4d212-559">array:entries:5</span></span> | <span data-ttu-id="4d212-560">value5</span><span class="sxs-lookup"><span data-stu-id="4d212-560">value5</span></span> |

<span data-ttu-id="4d212-561">Queste chiavi e i valori vengono caricati nell'app di esempio usando il provider di configurazione della memoria:</span><span class="sxs-lookup"><span data-stu-id="4d212-561">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

<span data-ttu-id="4d212-562">La matrice ignora un valore per l'indice &num;3.</span><span class="sxs-lookup"><span data-stu-id="4d212-562">The array skips a value for index &num;3.</span></span> <span data-ttu-id="4d212-563">Lo strumento di associazione di configurazione non è in grado di associare valori null o di creare voci null negli oggetti associati, situazione che diventerà chiara tra poco quando viene illustrato il risultato dell'associazione di questa matrice a un oggetto.</span><span class="sxs-lookup"><span data-stu-id="4d212-563">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="4d212-564">Nell'app di esempio, è disponibile una classe POCO per i dati di configurazione associati:</span><span class="sxs-lookup"><span data-stu-id="4d212-564">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="4d212-565">I dati di configurazione sono associati all'oggetto:</span><span class="sxs-lookup"><span data-stu-id="4d212-565">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="4d212-566">`GetSection` è incluso nel pacchetto [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), a sua volta incluso nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4d212-566">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="4d212-567">È possibile usare anche la sintassi [ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*), che produce codice più compatto:</span><span class="sxs-lookup"><span data-stu-id="4d212-567">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="4d212-568">L'oggetto associato, un'istanza di `ArrayExample`, riceve i dati di matrice dalla configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d212-568">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="4d212-569">Indice di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="4d212-569">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="4d212-570">Valore di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="4d212-570">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="4d212-571">0</span><span class="sxs-lookup"><span data-stu-id="4d212-571">0</span></span>                            | <span data-ttu-id="4d212-572">value0</span><span class="sxs-lookup"><span data-stu-id="4d212-572">value0</span></span>                       |
| <span data-ttu-id="4d212-573">1</span><span class="sxs-lookup"><span data-stu-id="4d212-573">1</span></span>                            | <span data-ttu-id="4d212-574">value1</span><span class="sxs-lookup"><span data-stu-id="4d212-574">value1</span></span>                       |
| <span data-ttu-id="4d212-575">2</span><span class="sxs-lookup"><span data-stu-id="4d212-575">2</span></span>                            | <span data-ttu-id="4d212-576">value2</span><span class="sxs-lookup"><span data-stu-id="4d212-576">value2</span></span>                       |
| <span data-ttu-id="4d212-577">3</span><span class="sxs-lookup"><span data-stu-id="4d212-577">3</span></span>                            | <span data-ttu-id="4d212-578">value4</span><span class="sxs-lookup"><span data-stu-id="4d212-578">value4</span></span>                       |
| <span data-ttu-id="4d212-579">4</span><span class="sxs-lookup"><span data-stu-id="4d212-579">4</span></span>                            | <span data-ttu-id="4d212-580">value5</span><span class="sxs-lookup"><span data-stu-id="4d212-580">value5</span></span>                       |

<span data-ttu-id="4d212-581">L'indice &num;3 nell'oggetto associato contiene i dati di configurazione per la chiave di configurazione `array:4` e il relativo valore `value4`.</span><span class="sxs-lookup"><span data-stu-id="4d212-581">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="4d212-582">Quando i dati di configurazione contenenti una matrice vengono associati, gli indici di matrice nelle chiavi di configurazione vengono usati semplicemente per scorrere i dati di configurazione quando si crea l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="4d212-582">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="4d212-583">Un valore null non può essere mantenuto nei dati di configurazione e una voce con valore null non viene creata in un oggetto associato quando una matrice nelle chiavi di configurazione ignora uno o più indici.</span><span class="sxs-lookup"><span data-stu-id="4d212-583">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="4d212-584">L'elemento di configurazione mancante per l'indice &num;3 può essere fornito prima dell'associazione all'istanza `ArrayExample` da qualsiasi provider di configurazione che produce la coppia chiave-valore corretta nella configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d212-584">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="4d212-585">Se il codice di esempio include un provider di configurazione JSON aggiuntivo con la coppia chiave-valore mancante, `ArrayExample.Entries` corrisponde alla matrice di configurazione completa:</span><span class="sxs-lookup"><span data-stu-id="4d212-585">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="4d212-586">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="4d212-586">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="4d212-587">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="4d212-587">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="4d212-588">La coppia chiave-valore mostrata nella tabella viene caricata nella configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d212-588">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="4d212-589">Chiave</span><span class="sxs-lookup"><span data-stu-id="4d212-589">Key</span></span>             | <span data-ttu-id="4d212-590">Value</span><span class="sxs-lookup"><span data-stu-id="4d212-590">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="4d212-591">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="4d212-591">array:entries:3</span></span> | <span data-ttu-id="4d212-592">value3</span><span class="sxs-lookup"><span data-stu-id="4d212-592">value3</span></span> |

<span data-ttu-id="4d212-593">Se l'istanza della classe `ArrayExample` viene associata dopo che il provider di configurazione JSON include la voce per l'indice &num;3, la matrice `ArrayExample.Entries` include il valore.</span><span class="sxs-lookup"><span data-stu-id="4d212-593">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="4d212-594">Indice di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="4d212-594">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="4d212-595">Valore di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="4d212-595">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="4d212-596">0</span><span class="sxs-lookup"><span data-stu-id="4d212-596">0</span></span>                            | <span data-ttu-id="4d212-597">value0</span><span class="sxs-lookup"><span data-stu-id="4d212-597">value0</span></span>                       |
| <span data-ttu-id="4d212-598">1</span><span class="sxs-lookup"><span data-stu-id="4d212-598">1</span></span>                            | <span data-ttu-id="4d212-599">value1</span><span class="sxs-lookup"><span data-stu-id="4d212-599">value1</span></span>                       |
| <span data-ttu-id="4d212-600">2</span><span class="sxs-lookup"><span data-stu-id="4d212-600">2</span></span>                            | <span data-ttu-id="4d212-601">value2</span><span class="sxs-lookup"><span data-stu-id="4d212-601">value2</span></span>                       |
| <span data-ttu-id="4d212-602">3</span><span class="sxs-lookup"><span data-stu-id="4d212-602">3</span></span>                            | <span data-ttu-id="4d212-603">value3</span><span class="sxs-lookup"><span data-stu-id="4d212-603">value3</span></span>                       |
| <span data-ttu-id="4d212-604">4</span><span class="sxs-lookup"><span data-stu-id="4d212-604">4</span></span>                            | <span data-ttu-id="4d212-605">value4</span><span class="sxs-lookup"><span data-stu-id="4d212-605">value4</span></span>                       |
| <span data-ttu-id="4d212-606">5</span><span class="sxs-lookup"><span data-stu-id="4d212-606">5</span></span>                            | <span data-ttu-id="4d212-607">value5</span><span class="sxs-lookup"><span data-stu-id="4d212-607">value5</span></span>                       |

<span data-ttu-id="4d212-608">**Elaborazione di matrice JSON**</span><span class="sxs-lookup"><span data-stu-id="4d212-608">**JSON array processing**</span></span>

<span data-ttu-id="4d212-609">Se un file JSON contiene una matrice, vengono create chiavi di configurazione per gli elementi della matrice con un indice di sezione in base zero.</span><span class="sxs-lookup"><span data-stu-id="4d212-609">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="4d212-610">Nel file di configurazione seguente, `subsection` è una matrice:</span><span class="sxs-lookup"><span data-stu-id="4d212-610">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="4d212-611">Il provider di configurazione JSON legge i dati di configurazione nelle coppie chiave-valore seguenti:</span><span class="sxs-lookup"><span data-stu-id="4d212-611">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="4d212-612">Chiave</span><span class="sxs-lookup"><span data-stu-id="4d212-612">Key</span></span>                     | <span data-ttu-id="4d212-613">Value</span><span class="sxs-lookup"><span data-stu-id="4d212-613">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="4d212-614">json_array:key</span><span class="sxs-lookup"><span data-stu-id="4d212-614">json_array:key</span></span>          | <span data-ttu-id="4d212-615">valueA</span><span class="sxs-lookup"><span data-stu-id="4d212-615">valueA</span></span> |
| <span data-ttu-id="4d212-616">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="4d212-616">json_array:subsection:0</span></span> | <span data-ttu-id="4d212-617">valueB</span><span class="sxs-lookup"><span data-stu-id="4d212-617">valueB</span></span> |
| <span data-ttu-id="4d212-618">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="4d212-618">json_array:subsection:1</span></span> | <span data-ttu-id="4d212-619">valueC</span><span class="sxs-lookup"><span data-stu-id="4d212-619">valueC</span></span> |
| <span data-ttu-id="4d212-620">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="4d212-620">json_array:subsection:2</span></span> | <span data-ttu-id="4d212-621">valueD</span><span class="sxs-lookup"><span data-stu-id="4d212-621">valueD</span></span> |

<span data-ttu-id="4d212-622">Nell'app di esempio, la classe POCO seguente è disponibile per associare le coppie chiave-valore della configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d212-622">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="4d212-623">Dopo l'associazione, `JsonArrayExample.Key` contiene il valore `valueA`.</span><span class="sxs-lookup"><span data-stu-id="4d212-623">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="4d212-624">I valori di sottosezione vengono archiviati nella proprietà matrice POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="4d212-624">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="4d212-625">Indice di `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="4d212-625">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="4d212-626">Valore di `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="4d212-626">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="4d212-627">0</span><span class="sxs-lookup"><span data-stu-id="4d212-627">0</span></span>                                   | <span data-ttu-id="4d212-628">valueB</span><span class="sxs-lookup"><span data-stu-id="4d212-628">valueB</span></span>                              |
| <span data-ttu-id="4d212-629">1</span><span class="sxs-lookup"><span data-stu-id="4d212-629">1</span></span>                                   | <span data-ttu-id="4d212-630">valueC</span><span class="sxs-lookup"><span data-stu-id="4d212-630">valueC</span></span>                              |
| <span data-ttu-id="4d212-631">2</span><span class="sxs-lookup"><span data-stu-id="4d212-631">2</span></span>                                   | <span data-ttu-id="4d212-632">valueD</span><span class="sxs-lookup"><span data-stu-id="4d212-632">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="4d212-633">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="4d212-633">Custom configuration provider</span></span>

<span data-ttu-id="4d212-634">L'app di esempio dimostra come creare un provider di configurazione di base che legge le coppie chiave-valore di configurazione da un database usando [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="4d212-634">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="4d212-635">Il provider ha le caratteristiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="4d212-635">The provider has the following characteristics:</span></span>

* <span data-ttu-id="4d212-636">Il database in memoria di Entity Framework viene usato a scopo dimostrativo.</span><span class="sxs-lookup"><span data-stu-id="4d212-636">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="4d212-637">Per usare un database che richiede una stringa di connessione, implementare un `ConfigurationBuilder` secondario per fornire la stringa di connessione da un altro provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d212-637">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="4d212-638">Il provider legge una tabella di database in una configurazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="4d212-638">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="4d212-639">Il provider non esegue una query sul database per ogni chiave.</span><span class="sxs-lookup"><span data-stu-id="4d212-639">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="4d212-640">Il ricaricamento in caso di modifica non è implementato, quindi l'aggiornamento del database dopo l'avvio dell'app non ha alcun effetto sulla configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="4d212-640">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="4d212-641">Definire un'entità `EFConfigurationValue` per l'archiviazione dei valori di configurazione nel database.</span><span class="sxs-lookup"><span data-stu-id="4d212-641">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="4d212-642">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d212-642">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="4d212-643">Aggiungere `EFConfigurationContext` per archiviare i valori configurati e accedervi.</span><span class="sxs-lookup"><span data-stu-id="4d212-643">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="4d212-644">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d212-644">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="4d212-645">Creare una classe che implementi <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="4d212-645">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="4d212-646">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d212-646">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="4d212-647">Creare il provider di configurazione personalizzato ereditando da <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="4d212-647">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="4d212-648">Il provider di configurazione inizializza il database quando è vuoto.</span><span class="sxs-lookup"><span data-stu-id="4d212-648">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="4d212-649">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d212-649">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="4d212-650">Un metodo di estensione `AddEFConfiguration` consente di aggiungere l'origine di configurazione a un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4d212-650">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="4d212-651">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d212-651">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="4d212-652">L'esempio di codice seguente mostra come usare il `EFConfigurationProvider` personalizzato in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d212-652">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="4d212-653">Accedere alla configurazione durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="4d212-653">Access configuration during startup</span></span>

<span data-ttu-id="4d212-654">Inserire `IConfiguration` nel costruttore `Startup` per accedere ai valori di configurazione in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4d212-654">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="4d212-655">Per accedere alla configurazione in `Startup.Configure`, inserire `IConfiguration` direttamente nel metodo o usare l'istanza dal costruttore:</span><span class="sxs-lookup"><span data-stu-id="4d212-655">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="4d212-656">Per un esempio di accesso alla configurazione usando metodi di servizio di avvio, vedere [Avvio dell'applicazione: Metodi pratici](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="4d212-656">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="4d212-657">Accedere alla configurazione in una pagina Razor Pages o in una visualizzazione MVC</span><span class="sxs-lookup"><span data-stu-id="4d212-657">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="4d212-658">Per accedere alle impostazioni di configurazione in una pagina Razor Pages o in una visualizzazione MVC, aggiungere un [direttiva using](xref:mvc/views/razor#using) ([Informazioni di riferimento su C#: direttiva using](/dotnet/csharp/language-reference/keywords/using-directive)) per lo [spazio dei nomi Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) e inserire <xref:Microsoft.Extensions.Configuration.IConfiguration> nella pagina o nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="4d212-658">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="4d212-659">In una pagina Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="4d212-659">In a Razor Pages page:</span></span>

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

<span data-ttu-id="4d212-660">In una visualizzazione MVC:</span><span class="sxs-lookup"><span data-stu-id="4d212-660">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="4d212-661">Aggiungere la configurazione da un assembly esterno</span><span class="sxs-lookup"><span data-stu-id="4d212-661">Add configuration from an external assembly</span></span>

<span data-ttu-id="4d212-662">Un'implementazione <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> consente l'aggiunta di miglioramenti a un'app all'avvio, da un assembly esterno alla classe `Startup` dell'app.</span><span class="sxs-lookup"><span data-stu-id="4d212-662">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="4d212-663">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="4d212-663">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4d212-664">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4d212-664">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* <span data-ttu-id="4d212-665">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/) (Configurazione Microsoft in dettaglio)</span><span class="sxs-lookup"><span data-stu-id="4d212-665">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)</span></span>
