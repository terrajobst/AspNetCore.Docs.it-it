---
title: Configurazione in ASP.NET Core
author: guardrex
description: Informazioni su come usare l'API di configurazione per configurare un'app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: a0c57e75b28bc7c5590d20a8fa59b00b6bb9af4e
ms.sourcegitcommit: 25150f4398de83132965a89f12d3a030f6cce48d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/25/2018
ms.locfileid: "42927878"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="28a23-103">Configurazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="28a23-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="28a23-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="28a23-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="28a23-105">La configurazione delle app in ASP.NET Core si basa su coppie chiave-valore stabilite dai *provider di configurazione*.</span><span class="sxs-lookup"><span data-stu-id="28a23-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="28a23-106">I provider di configurazione leggono i dati di configurazione in coppie chiave-valore da un'ampia gamma di origini di configurazione:</span><span class="sxs-lookup"><span data-stu-id="28a23-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="28a23-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="28a23-107">Azure Key Vault</span></span>
* <span data-ttu-id="28a23-108">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="28a23-108">Command-line arguments</span></span>
* <span data-ttu-id="28a23-109">Provider personalizzati (installati o creati)</span><span class="sxs-lookup"><span data-stu-id="28a23-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="28a23-110">File della directory</span><span class="sxs-lookup"><span data-stu-id="28a23-110">Directory files</span></span>
* <span data-ttu-id="28a23-111">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="28a23-111">Environment variables</span></span>
* <span data-ttu-id="28a23-112">Oggetti .NET in memoria</span><span class="sxs-lookup"><span data-stu-id="28a23-112">In-memory .NET objects</span></span>
* <span data-ttu-id="28a23-113">File di impostazioni</span><span class="sxs-lookup"><span data-stu-id="28a23-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="28a23-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="28a23-114">Azure Key Vault</span></span>
* <span data-ttu-id="28a23-115">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="28a23-115">Command-line arguments</span></span>
* <span data-ttu-id="28a23-116">Provider personalizzati (installati o creati)</span><span class="sxs-lookup"><span data-stu-id="28a23-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="28a23-117">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="28a23-117">Environment variables</span></span>
* <span data-ttu-id="28a23-118">Oggetti .NET in memoria</span><span class="sxs-lookup"><span data-stu-id="28a23-118">In-memory .NET objects</span></span>
* <span data-ttu-id="28a23-119">File di impostazioni</span><span class="sxs-lookup"><span data-stu-id="28a23-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="28a23-120">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="28a23-120">Command-line arguments</span></span>
* <span data-ttu-id="28a23-121">Provider personalizzati (installati o creati)</span><span class="sxs-lookup"><span data-stu-id="28a23-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="28a23-122">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="28a23-122">Environment variables</span></span>
* <span data-ttu-id="28a23-123">Oggetti .NET in memoria</span><span class="sxs-lookup"><span data-stu-id="28a23-123">In-memory .NET objects</span></span>
* <span data-ttu-id="28a23-124">File di impostazioni</span><span class="sxs-lookup"><span data-stu-id="28a23-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="28a23-125">Il *modello di opzioni* è un'estensione dei concetti di configurazione descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="28a23-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="28a23-126">Le opzioni usano le classi per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="28a23-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="28a23-127">Per altre informazioni sull'uso del modello di opzioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="28a23-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="28a23-128">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="28a23-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="28a23-129">Gli esempi in questo argomento si basano su:</span><span class="sxs-lookup"><span data-stu-id="28a23-129">The examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="28a23-130">Impostazione del percorso base dell'app con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="28a23-130">Setting the base path of the app with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="28a23-131">`SetBasePath` viene resa disponibile per un'app facendo riferimento al pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/).</span><span class="sxs-lookup"><span data-stu-id="28a23-131">`SetBasePath` is made available to an app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="28a23-132">Risoluzione di sezioni dei file di configurazione con <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span><span class="sxs-lookup"><span data-stu-id="28a23-132">Resolving sections of configuration files with <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span></span> <span data-ttu-id="28a23-133">`GetSection` viene resa disponibile per un'app facendo riferimento al pacchetto [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/).</span><span class="sxs-lookup"><span data-stu-id="28a23-133">`GetSection` is made available to an app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="28a23-134">Associazione della configurazione a classi .NET con <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> e [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span><span class="sxs-lookup"><span data-stu-id="28a23-134">Binding configuration to .NET classes with <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> and [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span></span> <span data-ttu-id="28a23-135">`Bind` e `Get<T>` vengono rese disponibili per un'app facendo riferimento al pacchetto [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/).</span><span class="sxs-lookup"><span data-stu-id="28a23-135">`Bind` and `Get<T>` are made available to an app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span> <span data-ttu-id="28a23-136">`Get<T>` è disponibile in ASP.NET Core 1.1 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="28a23-136">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="28a23-137">Questi tre pacchetti sono inclusi nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="28a23-137">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="28a23-138">Questi tre pacchetti sono inclusi nel [metapacchetto Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="28a23-138">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="28a23-139">Host e configurazione delle app</span><span class="sxs-lookup"><span data-stu-id="28a23-139">Host vs. app configuration</span></span>

<span data-ttu-id="28a23-140">Prima che l'app venga configurata e avviata, viene configurato e avviato un *host*.</span><span class="sxs-lookup"><span data-stu-id="28a23-140">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="28a23-141">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="28a23-141">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="28a23-142">Sia l'app che l'host vengono configurati tramite i provider di configurazione descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="28a23-142">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="28a23-143">Le coppie chiave-valore di configurazione dell'host diventano parte della configurazione globale dell'app.</span><span class="sxs-lookup"><span data-stu-id="28a23-143">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="28a23-144">Per altre informazioni su come vengono usati i provider di configurazione quando viene creato l'host e sugli effetti delle origini di configurazione sull'host di configurazione, vedere <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="28a23-144">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="security"></a><span data-ttu-id="28a23-145">Sicurezza</span><span class="sxs-lookup"><span data-stu-id="28a23-145">Security</span></span>

<span data-ttu-id="28a23-146">Adottare le procedure consigliate seguenti:</span><span class="sxs-lookup"><span data-stu-id="28a23-146">Adopt the following best practices:</span></span>

* <span data-ttu-id="28a23-147">Non archiviare mai la password o altri dati sensibili nel codice del provider di configurazione o in file di configurazione di testo normale.</span><span class="sxs-lookup"><span data-stu-id="28a23-147">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="28a23-148">Non usare i segreti di produzione in ambienti di sviluppo o di test.</span><span class="sxs-lookup"><span data-stu-id="28a23-148">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="28a23-149">Specificare i segreti all'esterno del progetto in modo che non possano essere inavvertitamente inviati a un repository del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="28a23-149">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="28a23-150">Altre informazioni su [come usare più ambienti](xref:fundamentals/environments) e la gestione dell'[archiviazione sicura dei segreti delle app durante lo sviluppo con Secret Manager](xref:security/app-secrets) (include consigli sull'uso delle variabili di ambiente per l'archiviazione di dati sensibili).</span><span class="sxs-lookup"><span data-stu-id="28a23-150">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="28a23-151">Secret Manager usa il provider di configurazione dei file per archiviare i segreti utente in un file JSON nel sistema locale.</span><span class="sxs-lookup"><span data-stu-id="28a23-151">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="28a23-152">Il provider di configurazione dei file è descritto più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="28a23-152">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="28a23-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) è una delle opzioni per l'archiviazione sicura dei segreti delle app.</span><span class="sxs-lookup"><span data-stu-id="28a23-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="28a23-154">Per ulteriori informazioni, vedere <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="28a23-154">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="28a23-155">Dati di configurazione gerarchici</span><span class="sxs-lookup"><span data-stu-id="28a23-155">Hierarchical configuration data</span></span>

<span data-ttu-id="28a23-156">L'API di configurazione è in grado di mantenere i dati di configurazione gerarchici rendendoli flat grazie all'uso di un delimitatore nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="28a23-156">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="28a23-157">Nel file JSON seguente, esistono quattro chiavi in una gerarchia strutturata di due sezioni:</span><span class="sxs-lookup"><span data-stu-id="28a23-157">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="28a23-158">Quando il file viene letto nella configurazione, vengono create chiavi univoche per mantenere la struttura di dati gerarchici originale dell'origine di configurazione.</span><span class="sxs-lookup"><span data-stu-id="28a23-158">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="28a23-159">Le sezioni e le chiavi vengono rese flat usando due punti (`:`) per mantenere la struttura originale:</span><span class="sxs-lookup"><span data-stu-id="28a23-159">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="28a23-160">section0:key0</span><span class="sxs-lookup"><span data-stu-id="28a23-160">section0:key0</span></span>
* <span data-ttu-id="28a23-161">section0:key1</span><span class="sxs-lookup"><span data-stu-id="28a23-161">section0:key1</span></span>
* <span data-ttu-id="28a23-162">section1:key0</span><span class="sxs-lookup"><span data-stu-id="28a23-162">section1:key0</span></span>
* <span data-ttu-id="28a23-163">section1:key1</span><span class="sxs-lookup"><span data-stu-id="28a23-163">section1:key1</span></span>

<span data-ttu-id="28a23-164">I metodi <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> e <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> sono disponibili per isolare le sezioni e gli elementi figlio di una sezione nei dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="28a23-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="28a23-165">Questi metodi sono descritti più avanti in [GetSection, GetChildren ed Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="28a23-165">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="28a23-166">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="28a23-166">Conventions</span></span>

<span data-ttu-id="28a23-167">All'avvio dell'app, le origini di configurazione vengono lette nell'ordine con cui sono specificati i rispettivi provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="28a23-167">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="28a23-168">I provider di configurazione dei file supportano il ricaricamento della configurazione quando un file di impostazioni sottostante viene modificato dopo l'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="28a23-168">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="28a23-169">Il provider di configurazione dei file è descritto più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="28a23-169">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="28a23-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> è disponibile nel contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) dell'app.</span><span class="sxs-lookup"><span data-stu-id="28a23-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="28a23-171">I provider di configurazione non possono usare l'inserimento delle dipendenze, perché non è disponibile quando vengono configurati dall'host.</span><span class="sxs-lookup"><span data-stu-id="28a23-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="28a23-172">Le chiavi di configurazione adottano le convenzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="28a23-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="28a23-173">Per le chiavi non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="28a23-173">Keys are case-insensitive.</span></span> <span data-ttu-id="28a23-174">Ad esempio, `ConnectionString` e `connectionstring` vengono considerate chiavi equivalenti.</span><span class="sxs-lookup"><span data-stu-id="28a23-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="28a23-175">Se un valore per la stessa chiave viene impostato dallo stesso provider di configurazione o da provider diversi, viene usato l'ultimo valore impostato per la chiave.</span><span class="sxs-lookup"><span data-stu-id="28a23-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="28a23-176">Chiavi gerarchiche</span><span class="sxs-lookup"><span data-stu-id="28a23-176">Hierarchical keys</span></span>
  * <span data-ttu-id="28a23-177">Nell'ambito dell'API di configurazione, il separatore due punti (`:`) funziona in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="28a23-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="28a23-178">Nelle variabili di ambiente, un separatore due punti potrebbe non funzionare in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="28a23-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="28a23-179">Il doppio carattere di sottolineatura (`__`) è supportato da tutte le piattaforme e viene convertito in due punti.</span><span class="sxs-lookup"><span data-stu-id="28a23-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="28a23-180">In Azure Key Vault, le chiavi gerarchiche usano `--` (due trattini) come separatore.</span><span class="sxs-lookup"><span data-stu-id="28a23-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="28a23-181">È necessario fornire il codice per sostituire i trattini con due punti quando i segreti vengono caricati nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="28a23-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="28a23-182">Il <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supporta l'associazione di matrici agli oggetti usando gli indici delle matrici nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="28a23-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="28a23-183">L'associazione di matrici è descritta nella sezione [Associare una matrice a una classe](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="28a23-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="28a23-184">I valori di configurazione adottano le convenzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="28a23-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="28a23-185">I valori sono stringhe.</span><span class="sxs-lookup"><span data-stu-id="28a23-185">Values are strings.</span></span>
* <span data-ttu-id="28a23-186">I valori null non possono essere archiviati nella configurazione o associati a oggetti.</span><span class="sxs-lookup"><span data-stu-id="28a23-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="28a23-187">Provider</span><span class="sxs-lookup"><span data-stu-id="28a23-187">Providers</span></span>

<span data-ttu-id="28a23-188">La tabella seguente mostra i provider di configurazione disponibili per le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="28a23-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="28a23-189">Provider</span><span class="sxs-lookup"><span data-stu-id="28a23-189">Provider</span></span> | <span data-ttu-id="28a23-190">Fornisce la configurazione da&hellip;</span><span class="sxs-lookup"><span data-stu-id="28a23-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="28a23-191">[Provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="28a23-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="28a23-192">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="28a23-192">Azure Key Vault</span></span> |
| [<span data-ttu-id="28a23-193">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="28a23-193">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="28a23-194">Parametri della riga di comando</span><span class="sxs-lookup"><span data-stu-id="28a23-194">Command-line parameters</span></span> |
| [<span data-ttu-id="28a23-195">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="28a23-195">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="28a23-196">Origine personalizzata</span><span class="sxs-lookup"><span data-stu-id="28a23-196">Custom source</span></span> |
| [<span data-ttu-id="28a23-197">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="28a23-197">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="28a23-198">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="28a23-198">Environment variables</span></span> |
| [<span data-ttu-id="28a23-199">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="28a23-199">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="28a23-200">File (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="28a23-200">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="28a23-201">Provider di configurazione chiave-per-file</span><span class="sxs-lookup"><span data-stu-id="28a23-201">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="28a23-202">File della directory</span><span class="sxs-lookup"><span data-stu-id="28a23-202">Directory files</span></span> |
| [<span data-ttu-id="28a23-203">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="28a23-203">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="28a23-204">Raccolte in memoria</span><span class="sxs-lookup"><span data-stu-id="28a23-204">In-memory collections</span></span> |
| <span data-ttu-id="28a23-205">[Segreti utente (Secret Manager)](xref:security/app-secrets) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="28a23-205">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="28a23-206">File nella directory dei profili utente</span><span class="sxs-lookup"><span data-stu-id="28a23-206">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="28a23-207">Provider</span><span class="sxs-lookup"><span data-stu-id="28a23-207">Provider</span></span> | <span data-ttu-id="28a23-208">Fornisce la configurazione da&hellip;</span><span class="sxs-lookup"><span data-stu-id="28a23-208">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="28a23-209">[Provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="28a23-209">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="28a23-210">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="28a23-210">Azure Key Vault</span></span> |
| [<span data-ttu-id="28a23-211">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="28a23-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="28a23-212">Parametri della riga di comando</span><span class="sxs-lookup"><span data-stu-id="28a23-212">Command-line parameters</span></span> |
| [<span data-ttu-id="28a23-213">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="28a23-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="28a23-214">Origine personalizzata</span><span class="sxs-lookup"><span data-stu-id="28a23-214">Custom source</span></span> |
| [<span data-ttu-id="28a23-215">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="28a23-215">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="28a23-216">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="28a23-216">Environment variables</span></span> |
| [<span data-ttu-id="28a23-217">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="28a23-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="28a23-218">File (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="28a23-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="28a23-219">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="28a23-219">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="28a23-220">Raccolte in memoria</span><span class="sxs-lookup"><span data-stu-id="28a23-220">In-memory collections</span></span> |
| <span data-ttu-id="28a23-221">[Segreti utente (Secret Manager)](xref:security/app-secrets) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="28a23-221">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="28a23-222">File nella directory dei profili utente</span><span class="sxs-lookup"><span data-stu-id="28a23-222">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="28a23-223">Provider</span><span class="sxs-lookup"><span data-stu-id="28a23-223">Provider</span></span> | <span data-ttu-id="28a23-224">Fornisce la configurazione da&hellip;</span><span class="sxs-lookup"><span data-stu-id="28a23-224">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="28a23-225">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="28a23-225">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="28a23-226">Parametri della riga di comando</span><span class="sxs-lookup"><span data-stu-id="28a23-226">Command-line parameters</span></span> |
| [<span data-ttu-id="28a23-227">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="28a23-227">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="28a23-228">Origine personalizzata</span><span class="sxs-lookup"><span data-stu-id="28a23-228">Custom source</span></span> |
| [<span data-ttu-id="28a23-229">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="28a23-229">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="28a23-230">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="28a23-230">Environment variables</span></span> |
| [<span data-ttu-id="28a23-231">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="28a23-231">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="28a23-232">File (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="28a23-232">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="28a23-233">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="28a23-233">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="28a23-234">Raccolte in memoria</span><span class="sxs-lookup"><span data-stu-id="28a23-234">In-memory collections</span></span> |
| <span data-ttu-id="28a23-235">[Segreti utente (Secret Manager)](xref:security/app-secrets) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="28a23-235">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="28a23-236">File nella directory dei profili utente</span><span class="sxs-lookup"><span data-stu-id="28a23-236">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="28a23-237">Le origini di configurazione vengono lette nell'ordine in cui vengono specificati i rispetti provider di configurazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="28a23-237">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="28a23-238">I provider di configurazione descritti in questo argomento vengono presentati in ordine alfabetico e non nell'ordine in cui potrebbe disporli il codice.</span><span class="sxs-lookup"><span data-stu-id="28a23-238">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="28a23-239">Ordinare i provider di configurazione nel codice in base alle specifiche priorità per le origini di configurazione sottostanti.</span><span class="sxs-lookup"><span data-stu-id="28a23-239">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="28a23-240">Una sequenza tipica di provider di configurazione è:</span><span class="sxs-lookup"><span data-stu-id="28a23-240">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="28a23-241">File (*appsettings.json*, *appsettings.&lt;Environment&gt;.json*, dove `<Environment>` è l'ambiente di hosting corrente dell'app)</span><span class="sxs-lookup"><span data-stu-id="28a23-241">Files (*appsettings.json*, *appsettings.&lt;Environment&gt;.json*, where `<Environment>` is the app's current hosting environment)</span></span>
1. <span data-ttu-id="28a23-242">[Segreti utente (Secret Manager)](xref:security/app-secrets) (sono nell'ambiente Development)</span><span class="sxs-lookup"><span data-stu-id="28a23-242">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="28a23-243">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="28a23-243">Environment variables</span></span>
1. <span data-ttu-id="28a23-244">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="28a23-244">Command-line arguments</span></span>

<span data-ttu-id="28a23-245">È pratica comune posizionare il provider di configurazione della riga di comando per ultimo in una serie di provider per consentire agli argomenti della riga di comando di sostituire la configurazione impostata da altri provider.</span><span class="sxs-lookup"><span data-stu-id="28a23-245">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="28a23-246">Questa sequenza di provider viene applicata quando si inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="28a23-246">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="28a23-247">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="28a23-247">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="28a23-248">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host Web per specificare i provider di configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="28a23-248">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the Web Host to specify the app's configuration providers:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

<span data-ttu-id="28a23-249">`ConfigureAppConfiguration`  *è disponibile in ASP.NET Core 2.1 o versioni successive.*</span><span class="sxs-lookup"><span data-stu-id="28a23-249">`ConfigureAppConfiguration` *is available in ASP.NET Core 2.1 or later.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="28a23-250">È possibile creare questa sequenza di provider per l'app (non l'host) con un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> e una chiamata al relativo metodo <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="28a23-250">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

```csharp
public Startup(IHostingEnvironment env)
{
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
        .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
            reloadOnChange: true);

    var appAssembly = Assembly.Load(new AssemblyName(env.ApplicationName));

    if (appAssembly != null)
    {
        builder.AddUserSecrets(appAssembly, optional: true);
    }

    builder.AddEnvironmentVariables();

    Configuration = builder.Build();
}

public IConfiguration Configuration { get; }

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IConfiguration>(Configuration);
}
```

<span data-ttu-id="28a23-251">Nell'esempio precedente, il nome dell'ambiente (`env.EnvironmentName`) e il nome di assembly dell'app (`env.ApplicationName`) vengono forniti da <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="28a23-251">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="28a23-252">Per ulteriori informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="28a23-252">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="28a23-253">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="28a23-253">Command-line Configuration Provider</span></span>

<span data-ttu-id="28a23-254"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> carica la configurazione da coppie chiave-valore di argomenti della riga di comando in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="28a23-254">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="28a23-255">Per attivare la configurazione della riga di comando, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="28a23-255">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="28a23-256">`AddCommandLine` viene chiamato automaticamente quando si Inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="28a23-256">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="28a23-257">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="28a23-257">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="28a23-258">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="28a23-258">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="28a23-259">Una configurazione facoltativa da *appsettings.json* e *appsettings.&lt;Environment&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="28a23-259">Optional configuration from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>
* <span data-ttu-id="28a23-260">[Segreti utente (Secret Manager)](xref:security/app-secrets) (nell'ambiente Development).</span><span class="sxs-lookup"><span data-stu-id="28a23-260">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="28a23-261">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="28a23-261">Environment variables.</span></span>

<span data-ttu-id="28a23-262">`CreateDefaultBuilder` aggiunge il provider di configurazione della riga di comando per ultimo.</span><span class="sxs-lookup"><span data-stu-id="28a23-262">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="28a23-263">Gli argomenti della riga di comando passati in fase di esecuzione sostituiscono la configurazione impostata dagli altri provider.</span><span class="sxs-lookup"><span data-stu-id="28a23-263">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="28a23-264">`CreateDefaultBuilder` opera durante la creazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="28a23-264">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="28a23-265">Pertanto, la configurazione della riga di comando attivata da `CreateDefaultBuilder` può influire sul modo in cui l'host viene configurato.</span><span class="sxs-lookup"><span data-stu-id="28a23-265">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="28a23-266">Quando si crea l'host manualmente e non si chiama `CreateDefaultBuilder`, chiamare il metodo di estensione `AddCommandLine` per un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="28a23-266">When building the host manually and not calling `CreateDefaultBuilder`, call the `AddCommandLine` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="28a23-267">Per attivare la configurazione della riga di comando, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="28a23-267">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="28a23-268">Chiamare il provider per ultimo per consentire agli argomenti della riga di comando passati in fase di esecuzione di sostituire la configurazione impostata da altri provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="28a23-268">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="28a23-269">Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="28a23-269">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="28a23-270">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="28a23-270">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="28a23-271">L'app di esempio 2.x consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include una chiamata a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="28a23-271">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="28a23-272">L'app di esempio 1.x chiama <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> per un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="28a23-272">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="28a23-273">Aprire un prompt dei comandi nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="28a23-273">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="28a23-274">Fornire un argomento della riga di comando per il comando `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="28a23-274">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="28a23-275">Dopo che l'app è in esecuzione, aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="28a23-275">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="28a23-276">Notare che l'output contiene la coppia chiave-valore per l'argomento della riga di comando di configurazione fornito per `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="28a23-276">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="28a23-277">Argomenti</span><span class="sxs-lookup"><span data-stu-id="28a23-277">Arguments</span></span>

<span data-ttu-id="28a23-278">Il valore deve seguire un segno di uguale (`=`) o la chiave deve avere un prefisso (`--` o `/`) quando il valore segue uno spazio.</span><span class="sxs-lookup"><span data-stu-id="28a23-278">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="28a23-279">Il valore può essere null se viene usato un segno di uguale (ad esempio, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="28a23-279">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="28a23-280">Prefisso chiave</span><span class="sxs-lookup"><span data-stu-id="28a23-280">Key prefix</span></span>               | <span data-ttu-id="28a23-281">Esempio</span><span class="sxs-lookup"><span data-stu-id="28a23-281">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="28a23-282">Nessun prefisso</span><span class="sxs-lookup"><span data-stu-id="28a23-282">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="28a23-283">Due trattini (`--`)</span><span class="sxs-lookup"><span data-stu-id="28a23-283">Two dashes (`--`)</span></span>        | <span data-ttu-id="28a23-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="28a23-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="28a23-285">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="28a23-285">Forward slash (`/`)</span></span>      | <span data-ttu-id="28a23-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="28a23-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="28a23-287">Nello stesso comando, non mischiare coppie chiave-valore di argomenti della riga di comando che usano un segno di uguale con coppie chiave-valore che usano uno spazio.</span><span class="sxs-lookup"><span data-stu-id="28a23-287">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="28a23-288">Comandi di esempio:</span><span class="sxs-lookup"><span data-stu-id="28a23-288">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value --CommandLineKey2=value /CommandLineKey2=value
dotnet run --CommandLineKey1 value /CommandLineKey2 value
dotnet run CommandLineKey1= CommandLineKey2=value
```

### <a name="switch-mappings"></a><span data-ttu-id="28a23-289">Mapping di sostituzione</span><span class="sxs-lookup"><span data-stu-id="28a23-289">Switch mappings</span></span>

<span data-ttu-id="28a23-290">I mapping di sostituzione consentono di usare la logica di sostituzione del nome della chiave.</span><span class="sxs-lookup"><span data-stu-id="28a23-290">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="28a23-291">Quando si crea manualmente la configurazione con un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, è possibile specificare un dizionario di mapping di sostituzione per il metodo <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="28a23-291">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="28a23-292">Quando viene utilizzato il dizionario dei mapping di sostituzione, nel dizionario viene controllata la presenza di una chiave corrispondente alla chiave fornita da un argomento della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="28a23-292">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="28a23-293">Se la chiave della riga di comando viene trovata nel dizionario, il valore del dizionario (la sostituzione della chiave) viene passato nuovamente per impostare la coppia chiave-valore nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="28a23-293">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="28a23-294">Un mapping di sostituzione è necessario per le chiavi della riga di comando con un trattino singolo (`-`) come prefisso.</span><span class="sxs-lookup"><span data-stu-id="28a23-294">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="28a23-295">Regole principali del dizionario dei mapping di sostituzione:</span><span class="sxs-lookup"><span data-stu-id="28a23-295">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="28a23-296">Le sostituzioni devono iniziare con un trattino (`-`) o un trattino doppio (`--`).</span><span class="sxs-lookup"><span data-stu-id="28a23-296">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="28a23-297">Il dizionario dei mapping di sostituzione non deve contenere chiavi duplicate.</span><span class="sxs-lookup"><span data-stu-id="28a23-297">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var switchMappings = new Dictionary<string, string>
            {
                { "-CLKey1", "CommandLineKey1" },
                { "-CLKey2", "CommandLineKey2" }
            };

        var config = new ConfigurationBuilder()
            .AddCommandLine(args, switchMappings)
            .Build();

        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="28a23-298">Come illustrato nell'esempio precedente, la chiamata a `CreateDefaultBuilder` non deve passare argomenti quando vengono usati i mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="28a23-298">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="28a23-299">La chiamata `AddCommandLine` del metodo `CreateDefaultBuilder` non include sostituzioni mappate e non è possibile passare il dizionario dei mapping di sostituzione a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="28a23-299">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="28a23-300">Se gli argomenti includono una sostituzione mappata e vengono passati a `CreateDefaultBuilder`, l'inizializzazione del rispettivo provider `AddCommandLine` non riesce con <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="28a23-300">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="28a23-301">La soluzione consiste nel non passare gli argomenti a `CreateDefaultBuilder` consentendo invece al metodo `AddCommandLine` del metodo `ConfigurationBuilder` di elaborare entrambi gli argomenti e il dizionario dei mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="28a23-301">The solution is not to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public static void Main(string[] args)
{
    var switchMappings = new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    var config = new ConfigurationBuilder()
        .AddCommandLine(args, switchMappings)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .UseStartup<Startup>()
        .Start();

    using (host)
    {
        Console.ReadLine();
    }
}
```

::: moniker-end

<span data-ttu-id="28a23-302">Il dizionario dei mapping di sostituzione creato contiene i dati visualizzati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="28a23-302">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="28a23-303">Chiave</span><span class="sxs-lookup"><span data-stu-id="28a23-303">Key</span></span>       | <span data-ttu-id="28a23-304">Valore</span><span class="sxs-lookup"><span data-stu-id="28a23-304">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="28a23-305">Se le chiavi con mapping di sostituzione vengono usate all'avvio dell'app, la configurazione riceve il valore di configurazione per la chiave fornita dal dizionario:</span><span class="sxs-lookup"><span data-stu-id="28a23-305">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="28a23-306">Dopo aver eseguito il comando precedente, la configurazione contiene i valori mostrati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="28a23-306">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="28a23-307">Chiave</span><span class="sxs-lookup"><span data-stu-id="28a23-307">Key</span></span>               | <span data-ttu-id="28a23-308">Valore</span><span class="sxs-lookup"><span data-stu-id="28a23-308">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="28a23-309">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="28a23-309">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="28a23-310"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> carica la configurazione da coppie chiave-valore di variabili di ambiente in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="28a23-310">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="28a23-311">Per attivare la configurazione delle variabili di ambiente, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="28a23-311">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="28a23-312">Quando si utilizzano chiavi gerarchiche in variabili di ambiente, il separatore due punti (`:`) potrebbe non funzionare in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="28a23-312">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="28a23-313">Il doppio carattere di sottolineatura (`__`) è supportato da tutte le piattaforme e viene sostituito con due punti.</span><span class="sxs-lookup"><span data-stu-id="28a23-313">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="28a23-314">[Servizio App di Azure](https://azure.microsoft.com/services/app-service/) consente di impostare variabili di ambiente nel portale di Azure che possono sostituire la configurazione delle app tramite il provider di configurazione delle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="28a23-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="28a23-315">Per altre informazioni, vedere [App di Azure: Eseguire l'override della configurazione delle app usando il portale di Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="28a23-315">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="28a23-316">`AddEnvironmentVariables` viene chiamato automaticamente quando si Inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="28a23-316">`AddEnvironmentVariables` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="28a23-317">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="28a23-317">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="28a23-318">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="28a23-318">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="28a23-319">Una configurazione facoltativa da *appsettings.json* e *appsettings.&lt;Environment&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="28a23-319">Optional configuration from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>
* <span data-ttu-id="28a23-320">[Segreti utente (Secret Manager)](xref:security/app-secrets) (nell'ambiente Development).</span><span class="sxs-lookup"><span data-stu-id="28a23-320">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="28a23-321">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="28a23-321">Command-line arguments.</span></span>

<span data-ttu-id="28a23-322">Il provider di configurazione delle variabili di ambiente viene chiamato dopo aver stabilito la configurazione dai segreti utente e dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="28a23-322">The Environment Variable Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="28a23-323">La chiamata del provider in questa posizione consente alle variabili di ambiente lette in fase di esecuzione di sostituire la configurazione impostata dai segreti utente e dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="28a23-323">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="28a23-324">È possibile chiamare direttamente il metodo di estensione `AddEnvironmentVariables` su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="28a23-324">You can also directly call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="28a23-325">Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="28a23-325">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="28a23-326">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="28a23-326">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="28a23-327">L'app di esempio 2.x consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include una chiamata a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="28a23-327">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="28a23-328">L'app di esempio 1.x chiama `AddEnvironmentVariables` per un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="28a23-328">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="28a23-329">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="28a23-329">Run the sample app.</span></span> <span data-ttu-id="28a23-330">Aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="28a23-330">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="28a23-331">Notare che l'output contiene la coppia chiave-valore per la variabile di ambiente `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="28a23-331">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="28a23-332">Il valore riflette l'ambiente in cui l'app è in esecuzione, in genere `Development` durante l'esecuzione locale.</span><span class="sxs-lookup"><span data-stu-id="28a23-332">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="28a23-333">Per mantenere breve l'elenco delle variabili di ambiente restituito dall'app, l'app filtra le variabili di ambiente includendo quelle che iniziano come segue:</span><span class="sxs-lookup"><span data-stu-id="28a23-333">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="28a23-334">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="28a23-334">ASPNETCORE_</span></span>
* <span data-ttu-id="28a23-335">urls</span><span class="sxs-lookup"><span data-stu-id="28a23-335">urls</span></span>
* <span data-ttu-id="28a23-336">Registrazione</span><span class="sxs-lookup"><span data-stu-id="28a23-336">Logging</span></span>
* <span data-ttu-id="28a23-337">ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="28a23-337">ENVIRONMENT</span></span>
* <span data-ttu-id="28a23-338">contentRoot</span><span class="sxs-lookup"><span data-stu-id="28a23-338">contentRoot</span></span>
* <span data-ttu-id="28a23-339">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="28a23-339">AllowedHosts</span></span>
* <span data-ttu-id="28a23-340">applicationName</span><span class="sxs-lookup"><span data-stu-id="28a23-340">applicationName</span></span>
* <span data-ttu-id="28a23-341">CommandLine</span><span class="sxs-lookup"><span data-stu-id="28a23-341">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="28a23-342">Se si desidera esporre tutte le variabili di ambiente disponibili all'app, modificare `FilteredConfiguration` in *Pages/Index.cshtml.cs* come segue:</span><span class="sxs-lookup"><span data-stu-id="28a23-342">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="28a23-343">Se si desidera esporre tutte le variabili di ambiente disponibili all'app, modificare `FilteredConfiguration` in *Controllers/HomeController.cs* come segue:</span><span class="sxs-lookup"><span data-stu-id="28a23-343">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="28a23-344">Prefissi</span><span class="sxs-lookup"><span data-stu-id="28a23-344">Prefixes</span></span>

<span data-ttu-id="28a23-345">Le variabili di ambiente caricate nella configurazione dell'app vengono filtrate quando si specifica un prefisso per il metodo `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="28a23-345">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="28a23-346">Ad esempio, per filtrare le variabili di ambiente in base al prefisso `CUSTOM_`, fornire il prefisso al provider di configurazione:</span><span class="sxs-lookup"><span data-stu-id="28a23-346">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="28a23-347">Il prefisso viene rimosso quando vengono create le coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="28a23-347">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="28a23-348">Il metodo di servizio statico `CreateDefaultBuilder` crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> per stabilire l'host dell'app.</span><span class="sxs-lookup"><span data-stu-id="28a23-348">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="28a23-349">Quando viene creato `WebHostBuilder`, la configurazione dell'host corrispondente viene individuata nelle variabili di ambiente con il prefisso `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="28a23-349">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="28a23-350">**Prefissi della stringa di connessione**</span><span class="sxs-lookup"><span data-stu-id="28a23-350">**Connection string prefixes**</span></span>

<span data-ttu-id="28a23-351">L'API di configurazione prevede regole di elaborazione speciali per quattro variabili di ambiente di stringa di connessione coinvolte nella configurazione di stringhe di connessione di Azure per l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="28a23-351">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="28a23-352">Le variabili di ambiente con i prefissi indicati nella tabella vengono caricate nell'app se non viene fornito alcun prefisso a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="28a23-352">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="28a23-353">Prefisso della stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="28a23-353">Connection string prefix</span></span> | <span data-ttu-id="28a23-354">Provider</span><span class="sxs-lookup"><span data-stu-id="28a23-354">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="28a23-355">Provider personalizzato</span><span class="sxs-lookup"><span data-stu-id="28a23-355">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="28a23-356">MySQL</span><span class="sxs-lookup"><span data-stu-id="28a23-356">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="28a23-357">Database SQL di Azure</span><span class="sxs-lookup"><span data-stu-id="28a23-357">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="28a23-358">SQL Server</span><span class="sxs-lookup"><span data-stu-id="28a23-358">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="28a23-359">Quando una variabile di ambiente viene individuata e caricata nella configurazione con uno qualsiasi dei quattro prefissi indicati nella tabella:</span><span class="sxs-lookup"><span data-stu-id="28a23-359">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="28a23-360">La chiave di configurazione viene creata rimuovendo il prefisso della variabile di ambiente e aggiungendo una sezione per la chiave di configurazione (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="28a23-360">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="28a23-361">Viene creata una nuova coppia chiave-valore della configurazione che rappresenta il provider di connessione al database (ad eccezione di `CUSTOMCONNSTR_`, che non ha un provider dichiarato).</span><span class="sxs-lookup"><span data-stu-id="28a23-361">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="28a23-362">Chiave della variabile di ambiente</span><span class="sxs-lookup"><span data-stu-id="28a23-362">Environment variable key</span></span> | <span data-ttu-id="28a23-363">Chiave di configurazione convertita</span><span class="sxs-lookup"><span data-stu-id="28a23-363">Converted configuration key</span></span> | <span data-ttu-id="28a23-364">Voce di configurazione del provider</span><span class="sxs-lookup"><span data-stu-id="28a23-364">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="28a23-365">Voce di configurazione non creata.</span><span class="sxs-lookup"><span data-stu-id="28a23-365">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="28a23-366">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="28a23-366">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="28a23-367">Valore: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="28a23-367">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="28a23-368">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="28a23-368">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="28a23-369">Valore: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="28a23-369">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="28a23-370">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="28a23-370">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="28a23-371">Valore: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="28a23-371">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="28a23-372">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="28a23-372">File Configuration Provider</span></span>

<span data-ttu-id="28a23-373"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> è la classe base per il caricamento della configurazione dal file system.</span><span class="sxs-lookup"><span data-stu-id="28a23-373"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="28a23-374">I provider di configurazione seguenti sono dedicati per specifici tipi di file:</span><span class="sxs-lookup"><span data-stu-id="28a23-374">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="28a23-375">Provider di configurazione INI</span><span class="sxs-lookup"><span data-stu-id="28a23-375">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="28a23-376">Provider di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="28a23-376">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="28a23-377">Provider di configurazione XML</span><span class="sxs-lookup"><span data-stu-id="28a23-377">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="28a23-378">Provider di configurazione INI</span><span class="sxs-lookup"><span data-stu-id="28a23-378">INI Configuration Provider</span></span>

<span data-ttu-id="28a23-379"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> carica la configurazione da coppie chiave-valore di file INI in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="28a23-379">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="28a23-380">Per attivare la configurazione di file INI, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="28a23-380">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="28a23-381">I due punti possono essere usati come delimitatore di sezione nella configurazione di file INI.</span><span class="sxs-lookup"><span data-stu-id="28a23-381">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="28a23-382">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="28a23-382">Overloads permit specifying:</span></span>

* <span data-ttu-id="28a23-383">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="28a23-383">Whether the file is optional.</span></span>
* <span data-ttu-id="28a23-384">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="28a23-384">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="28a23-385">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="28a23-385">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="28a23-386">Quando si chiama `CreateDefaultBuilder`, chiamare `UseConfiguration` con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="28a23-386">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddIniFile("config.ini", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="28a23-387">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare `UseConfiguration` con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="28a23-387">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="28a23-388">Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="28a23-388">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

::: moniker-end

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

<span data-ttu-id="28a23-389">Un esempio generico di un file di configurazione INI:</span><span class="sxs-lookup"><span data-stu-id="28a23-389">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="28a23-390">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="28a23-390">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="28a23-391">section0:key0</span><span class="sxs-lookup"><span data-stu-id="28a23-391">section0:key0</span></span>
* <span data-ttu-id="28a23-392">section0:key1</span><span class="sxs-lookup"><span data-stu-id="28a23-392">section0:key1</span></span>
* <span data-ttu-id="28a23-393">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="28a23-393">section1:subsection:key</span></span>
* <span data-ttu-id="28a23-394">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="28a23-394">section2:subsection0:key</span></span>
* <span data-ttu-id="28a23-395">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="28a23-395">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="28a23-396">Provider di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="28a23-396">JSON Configuration Provider</span></span>

<span data-ttu-id="28a23-397">Il <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> carica la configurazione da coppie chiave-valore di file JSON in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="28a23-397">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="28a23-398">Per attivare la configurazione di file JSON, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="28a23-398">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="28a23-399">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="28a23-399">Overloads permit specifying:</span></span>

* <span data-ttu-id="28a23-400">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="28a23-400">Whether the file is optional.</span></span>
* <span data-ttu-id="28a23-401">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="28a23-401">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="28a23-402">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="28a23-402">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="28a23-403">`AddJsonFile` viene chiamato automaticamente due volte quando si inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="28a23-403">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="28a23-404">Il metodo viene chiamato per caricare la configurazione da:</span><span class="sxs-lookup"><span data-stu-id="28a23-404">The method is called to load configuration from:</span></span>

* <span data-ttu-id="28a23-405">*appsettings.json* &ndash; Questo file viene letto per primo.</span><span class="sxs-lookup"><span data-stu-id="28a23-405">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="28a23-406">La versione dell'ambiente del file può sostituire i valori forniti dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="28a23-406">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="28a23-407">*appsettings.&lt;Environment&gt;.json* &ndash; La versione dell'ambiente del file viene caricata in base a [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="28a23-407">*appsettings.&lt;Environment&gt;.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="28a23-408">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="28a23-408">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="28a23-409">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="28a23-409">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="28a23-410">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="28a23-410">Environment variables.</span></span>
* <span data-ttu-id="28a23-411">[Segreti utente (Secret Manager)](xref:security/app-secrets) (nell'ambiente Development).</span><span class="sxs-lookup"><span data-stu-id="28a23-411">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="28a23-412">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="28a23-412">Command-line arguments.</span></span>

<span data-ttu-id="28a23-413">Il provider di configurazione JSON viene stabilito per primo.</span><span class="sxs-lookup"><span data-stu-id="28a23-413">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="28a23-414">I segreti utente, le variabili di ambiente e gli argomenti della riga di comando sostituiscono quindi la configurazione impostata dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="28a23-414">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="28a23-415">È possibile chiamare direttamente il metodo di estensione `AddJsonFile` su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="28a23-415">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="28a23-416">Quando si chiama `CreateDefaultBuilder`, chiamare `UseConfiguration` con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="28a23-416">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("config.json", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="28a23-417">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare `UseConfiguration` con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="28a23-417">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="28a23-418">Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="28a23-418">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

::: moniker-end

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

<span data-ttu-id="28a23-419">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="28a23-419">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="28a23-420">L'app di esempio 2.x consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include due chiamate a `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="28a23-420">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="28a23-421">La configurazione viene caricata da *appsettings.json* e *appsettings.&lt;Environment&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="28a23-421">Configuration is loaded from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="28a23-422">L'app di esempio 1.x chiama `AddJsonFile` due volte su un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="28a23-422">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="28a23-423">La configurazione viene caricata da *appsettings.json* e *appsettings.&lt;Environment&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="28a23-423">Configuration is loaded from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="28a23-424">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="28a23-424">Run the sample app.</span></span> <span data-ttu-id="28a23-425">Aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="28a23-425">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="28a23-426">Notare che l'output contiene coppie chiave-valore per la configurazione mostrata nella tabella in base all'ambiente.</span><span class="sxs-lookup"><span data-stu-id="28a23-426">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="28a23-427">Le chiavi di configurazione di registrazione usano i due punti (`:`) come separatore gerarchico.</span><span class="sxs-lookup"><span data-stu-id="28a23-427">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="28a23-428">Chiave</span><span class="sxs-lookup"><span data-stu-id="28a23-428">Key</span></span>                        | <span data-ttu-id="28a23-429">Valore Development</span><span class="sxs-lookup"><span data-stu-id="28a23-429">Development Value</span></span> | <span data-ttu-id="28a23-430">Valore Production</span><span class="sxs-lookup"><span data-stu-id="28a23-430">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="28a23-431">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="28a23-431">Logging:LogLevel:System</span></span>    | <span data-ttu-id="28a23-432">Informazioni</span><span class="sxs-lookup"><span data-stu-id="28a23-432">Information</span></span>       | <span data-ttu-id="28a23-433">Informazioni</span><span class="sxs-lookup"><span data-stu-id="28a23-433">Information</span></span>      |
| <span data-ttu-id="28a23-434">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="28a23-434">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="28a23-435">Informazioni</span><span class="sxs-lookup"><span data-stu-id="28a23-435">Information</span></span>       | <span data-ttu-id="28a23-436">Informazioni</span><span class="sxs-lookup"><span data-stu-id="28a23-436">Information</span></span>      |
| <span data-ttu-id="28a23-437">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="28a23-437">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="28a23-438">Debug</span><span class="sxs-lookup"><span data-stu-id="28a23-438">Debug</span></span>             | <span data-ttu-id="28a23-439">Error</span><span class="sxs-lookup"><span data-stu-id="28a23-439">Error</span></span>            |
| <span data-ttu-id="28a23-440">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="28a23-440">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="28a23-441">Provider di configurazione XML</span><span class="sxs-lookup"><span data-stu-id="28a23-441">XML Configuration Provider</span></span>

<span data-ttu-id="28a23-442">Il <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> carica la configurazione da coppie chiave-valore di file XML in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="28a23-442">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="28a23-443">Per attivare la configurazione di file XML, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="28a23-443">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="28a23-444">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="28a23-444">Overloads permit specifying:</span></span>

* <span data-ttu-id="28a23-445">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="28a23-445">Whether the file is optional.</span></span>
* <span data-ttu-id="28a23-446">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="28a23-446">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="28a23-447">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="28a23-447">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="28a23-448">Il nodo radice del file di configurazione viene ignorato quando vengono create le coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="28a23-448">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="28a23-449">Non specificare una definizione DTD (Document Type Definition) o uno spazio dei nomi nel file.</span><span class="sxs-lookup"><span data-stu-id="28a23-449">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="28a23-450">Quando si chiama `CreateDefaultBuilder`, chiamare `UseConfiguration` con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="28a23-450">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="28a23-451">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare `UseConfiguration` con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="28a23-451">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="28a23-452">Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="28a23-452">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

::: moniker-end

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

<span data-ttu-id="28a23-453">I file di configurazione XML possono usare nomi di elementi distinti per le sezioni ripetute:</span><span class="sxs-lookup"><span data-stu-id="28a23-453">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="28a23-454">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="28a23-454">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="28a23-455">section0:key0</span><span class="sxs-lookup"><span data-stu-id="28a23-455">section0:key0</span></span>
* <span data-ttu-id="28a23-456">section0:key1</span><span class="sxs-lookup"><span data-stu-id="28a23-456">section0:key1</span></span>
* <span data-ttu-id="28a23-457">section1:key0</span><span class="sxs-lookup"><span data-stu-id="28a23-457">section1:key0</span></span>
* <span data-ttu-id="28a23-458">section1:key1</span><span class="sxs-lookup"><span data-stu-id="28a23-458">section1:key1</span></span>

<span data-ttu-id="28a23-459">La ripetizione di elementi che usano lo stesso nome di elemento funziona se si usa l'attributo `name` per distinguere gli elementi:</span><span class="sxs-lookup"><span data-stu-id="28a23-459">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="28a23-460">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="28a23-460">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="28a23-461">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="28a23-461">section:section0:key:key0</span></span>
* <span data-ttu-id="28a23-462">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="28a23-462">section:section0:key:key1</span></span>
* <span data-ttu-id="28a23-463">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="28a23-463">section:section1:key:key0</span></span>
* <span data-ttu-id="28a23-464">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="28a23-464">section:section1:key:key1</span></span>

<span data-ttu-id="28a23-465">È possibile usare attributi per fornire valori:</span><span class="sxs-lookup"><span data-stu-id="28a23-465">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="28a23-466">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="28a23-466">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="28a23-467">key:attribute</span><span class="sxs-lookup"><span data-stu-id="28a23-467">key:attribute</span></span>
* <span data-ttu-id="28a23-468">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="28a23-468">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="28a23-469">Provider di configurazione KeyPerFile</span><span class="sxs-lookup"><span data-stu-id="28a23-469">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="28a23-470">Il <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> usa i file di una directory come coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="28a23-470">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="28a23-471">La chiave è il nome del file.</span><span class="sxs-lookup"><span data-stu-id="28a23-471">The key is the file name.</span></span> <span data-ttu-id="28a23-472">Il valore contiene il contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="28a23-472">The value contains the file's contents.</span></span> <span data-ttu-id="28a23-473">Il provider di configurazione KeyPerFile viene usato in scenari di hosting Docker.</span><span class="sxs-lookup"><span data-stu-id="28a23-473">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="28a23-474">Per attivare la configurazione chiave-per-file, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="28a23-474">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="28a23-475">Il `directoryPath` per i file deve essere un percorso assoluto.</span><span class="sxs-lookup"><span data-stu-id="28a23-475">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="28a23-476">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="28a23-476">Overloads permit specifying:</span></span>

* <span data-ttu-id="28a23-477">Un delegato `Action<KeyPerFileConfigurationSource>` che configura l'origine.</span><span class="sxs-lookup"><span data-stu-id="28a23-477">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="28a23-478">Se la directory è facoltativa e il percorso della directory.</span><span class="sxs-lookup"><span data-stu-id="28a23-478">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="28a23-479">Quando si chiama `CreateDefaultBuilder`, chiamare `UseConfiguration` con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="28a23-479">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
        var config = new ConfigurationBuilder()
            .AddKeyPerFile(directoryPath: path, optional: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="28a23-480">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare `UseConfiguration` con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="28a23-480">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

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

::: moniker-end

## <a name="memory-configuration-provider"></a><span data-ttu-id="28a23-481">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="28a23-481">Memory Configuration Provider</span></span>

<span data-ttu-id="28a23-482">Il <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> usa una raccolta in memoria come coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="28a23-482">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="28a23-483">Per attivare la configurazione della raccolta in memoria, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="28a23-483">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="28a23-484">Il provider di configurazione può essere inizializzato con un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="28a23-484">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="28a23-485">Quando si chiama `CreateDefaultBuilder`, chiamare `UseConfiguration` con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="28a23-485">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

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
}
```

<span data-ttu-id="28a23-486">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare `UseConfiguration` con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="28a23-486">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="28a23-487">Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="28a23-487">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

::: moniker-end

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

## <a name="getvalue"></a><span data-ttu-id="28a23-488">GetValue</span><span class="sxs-lookup"><span data-stu-id="28a23-488">GetValue</span></span>

<span data-ttu-id="28a23-489">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) estrae un valore dalla configurazione con una chiave specificata e lo converte nel tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="28a23-489">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="28a23-490">Un overload consente di specificare un valore predefinito se non viene trovata la chiave.</span><span class="sxs-lookup"><span data-stu-id="28a23-490">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="28a23-491">L'esempio seguente estrae il valore della stringa dalla configurazione con la chiave `NumberKey`, assegna al valore il tipo `int` e archivia il valore nella variabile `intValue`.</span><span class="sxs-lookup"><span data-stu-id="28a23-491">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="28a23-492">Se `NumberKey` non viene trovato nelle chiavi di configurazione `intValue` riceve il valore predefinito `99`:</span><span class="sxs-lookup"><span data-stu-id="28a23-492">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="28a23-493">GetSection, GetChildren ed Exists</span><span class="sxs-lookup"><span data-stu-id="28a23-493">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="28a23-494">Per gli esempi che seguono, prendere in considerazione il file JSON seguente.</span><span class="sxs-lookup"><span data-stu-id="28a23-494">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="28a23-495">Vengono trovate quattro chiavi in due sezioni, una delle quali include una coppia di sottosezioni:</span><span class="sxs-lookup"><span data-stu-id="28a23-495">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="28a23-496">Quando il file viene letto nella configurazione, vengono create le chiavi gerarchiche univoche seguenti per contenere i valori di configurazione:</span><span class="sxs-lookup"><span data-stu-id="28a23-496">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="28a23-497">section0:key0</span><span class="sxs-lookup"><span data-stu-id="28a23-497">section0:key0</span></span>
* <span data-ttu-id="28a23-498">section0:key1</span><span class="sxs-lookup"><span data-stu-id="28a23-498">section0:key1</span></span>
* <span data-ttu-id="28a23-499">section1:key0</span><span class="sxs-lookup"><span data-stu-id="28a23-499">section1:key0</span></span>
* <span data-ttu-id="28a23-500">section1:key1</span><span class="sxs-lookup"><span data-stu-id="28a23-500">section1:key1</span></span>
* <span data-ttu-id="28a23-501">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="28a23-501">section2:subsection0:key0</span></span>
* <span data-ttu-id="28a23-502">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="28a23-502">section2:subsection0:key1</span></span>
* <span data-ttu-id="28a23-503">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="28a23-503">section2:subsection1:key0</span></span>
* <span data-ttu-id="28a23-504">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="28a23-504">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="28a23-505">GetSection</span><span class="sxs-lookup"><span data-stu-id="28a23-505">GetSection</span></span>

<span data-ttu-id="28a23-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) estrae una sottosezione della configurazione con la chiave di sottosezione specificata.</span><span class="sxs-lookup"><span data-stu-id="28a23-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="28a23-507">Per restituire una <xref:Microsoft.Extensions.Configuration.IConfigurationSection> che contiene solo le coppie chiave-valore in `section1`, chiamare `GetSection` e fornire il nome della sezione:</span><span class="sxs-lookup"><span data-stu-id="28a23-507">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="28a23-508">In modo analogo, per ottenere i valori per le chiavi in `section2:subsection0`, chiamare `GetSection` e fornire il percorso della sezione:</span><span class="sxs-lookup"><span data-stu-id="28a23-508">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="28a23-509">`GetSection` non restituisce mai `null`.</span><span class="sxs-lookup"><span data-stu-id="28a23-509">`GetSection` never returns `null`.</span></span> <span data-ttu-id="28a23-510">Se non viene trovata una sezione corrispondente, viene restituita una `IConfigurationSection` vuota.</span><span class="sxs-lookup"><span data-stu-id="28a23-510">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

### <a name="getchildren"></a><span data-ttu-id="28a23-511">GetChildren</span><span class="sxs-lookup"><span data-stu-id="28a23-511">GetChildren</span></span>

<span data-ttu-id="28a23-512">Una chiamata a [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) per `section2` ottiene una `IEnumerable<IConfigurationSection>` che include:</span><span class="sxs-lookup"><span data-stu-id="28a23-512">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="28a23-513">Exists</span><span class="sxs-lookup"><span data-stu-id="28a23-513">Exists</span></span>

<span data-ttu-id="28a23-514">Usare [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) per determinare se esiste una sezione di configurazione:</span><span class="sxs-lookup"><span data-stu-id="28a23-514">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="28a23-515">Considerati i dati di esempio, `sectionExists` è `false` perché non esiste una sezione `section2:subsection2` nei dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="28a23-515">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="28a23-516">Eseguire l'associazione a una classe</span><span class="sxs-lookup"><span data-stu-id="28a23-516">Bind to a class</span></span>

<span data-ttu-id="28a23-517">La configurazione può essere associata alle classi che rappresentano gruppi di impostazioni correlate usando il *modello di opzioni*.</span><span class="sxs-lookup"><span data-stu-id="28a23-517">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="28a23-518">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="28a23-518">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="28a23-519">I valori di configurazione vengono restituiti come stringhe, ma la chiamata di <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> consente la costruzione di oggetti [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="28a23-519">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="28a23-520">L'app di esempio contiene un modello `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="28a23-520">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="28a23-521">La sezione `starship` del file *starship.json* crea la configurazione quando l'app di esempio usa il provider di configurazione JSON per caricare la configurazione:</span><span class="sxs-lookup"><span data-stu-id="28a23-521">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="28a23-522">Vengono create le coppie chiave-valore della configurazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="28a23-522">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="28a23-523">Chiave</span><span class="sxs-lookup"><span data-stu-id="28a23-523">Key</span></span>                   | <span data-ttu-id="28a23-524">Valore</span><span class="sxs-lookup"><span data-stu-id="28a23-524">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="28a23-525">starship:name</span><span class="sxs-lookup"><span data-stu-id="28a23-525">starship:name</span></span>         | <span data-ttu-id="28a23-526">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="28a23-526">USS Enterprise</span></span>                                    |
| <span data-ttu-id="28a23-527">starship:registry</span><span class="sxs-lookup"><span data-stu-id="28a23-527">starship:registry</span></span>     | <span data-ttu-id="28a23-528">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="28a23-528">NCC-1701</span></span>                                          |
| <span data-ttu-id="28a23-529">starship:class</span><span class="sxs-lookup"><span data-stu-id="28a23-529">starship:class</span></span>        | <span data-ttu-id="28a23-530">Constitution</span><span class="sxs-lookup"><span data-stu-id="28a23-530">Constitution</span></span>                                      |
| <span data-ttu-id="28a23-531">starship:length</span><span class="sxs-lookup"><span data-stu-id="28a23-531">starship:length</span></span>       | <span data-ttu-id="28a23-532">304.8</span><span class="sxs-lookup"><span data-stu-id="28a23-532">304.8</span></span>                                             |
| <span data-ttu-id="28a23-533">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="28a23-533">starship:commissioned</span></span> | <span data-ttu-id="28a23-534">False</span><span class="sxs-lookup"><span data-stu-id="28a23-534">False</span></span>                                             |
| <span data-ttu-id="28a23-535">trademark</span><span class="sxs-lookup"><span data-stu-id="28a23-535">trademark</span></span>             | <span data-ttu-id="28a23-536">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="28a23-536">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="28a23-537">L'app di esempio chiama `GetSection` con la chiave `starship`.</span><span class="sxs-lookup"><span data-stu-id="28a23-537">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="28a23-538">Le coppie chiave-valore `starship` sono isolate.</span><span class="sxs-lookup"><span data-stu-id="28a23-538">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="28a23-539">Il metodo `Bind` viene chiamato sulla sottosezione passando un'istanza della classe `Starship`.</span><span class="sxs-lookup"><span data-stu-id="28a23-539">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="28a23-540">Dopo aver associato i valori dell'istanza, l'istanza viene assegnata a una proprietà per il rendering:</span><span class="sxs-lookup"><span data-stu-id="28a23-540">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="28a23-541">Associazione a un oggetto grafico</span><span class="sxs-lookup"><span data-stu-id="28a23-541">Bind to an object graph</span></span>

<span data-ttu-id="28a23-542"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> è in grado di associare un intero oggetto grafico POCO.</span><span class="sxs-lookup"><span data-stu-id="28a23-542"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="28a23-543">L'esempio contiene un modello `TvShow` il cui oggetto grafico include `Metadata` e le classi `Actors` (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="28a23-543">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="28a23-544">L'app di esempio ha un file *tvshow.xml* che contiene i dati di configurazione:</span><span class="sxs-lookup"><span data-stu-id="28a23-544">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="28a23-545">La configurazione viene associata all'intero oggetto grafico `TvShow` con il metodo `Bind`.</span><span class="sxs-lookup"><span data-stu-id="28a23-545">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="28a23-546">L'istanza associata viene assegnata a una proprietà per il rendering:</span><span class="sxs-lookup"><span data-stu-id="28a23-546">The bound instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
viewModel.TvShow = tvShow;
```

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="28a23-547">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) associa e restituisce il tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="28a23-547">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="28a23-548">`Get<T>` può essere più comodo che usare `Bind`.</span><span class="sxs-lookup"><span data-stu-id="28a23-548">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="28a23-549">Il codice seguente mostra come usare `Get<T>` con l'esempio precedente, che consente di assegnare l'istanza associata direttamente alla proprietà usata per il rendering:</span><span class="sxs-lookup"><span data-stu-id="28a23-549">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="28a23-550">Associare una matrice a una classe</span><span class="sxs-lookup"><span data-stu-id="28a23-550">Bind an array to a class</span></span>

<span data-ttu-id="28a23-551">*L'app di esempio dimostra i concetti spiegati in questa sezione.*</span><span class="sxs-lookup"><span data-stu-id="28a23-551">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="28a23-552">Il <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supporta l'associazione di matrici agli oggetti usando gli indici delle matrici nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="28a23-552">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="28a23-553">Qualsiasi formato di matrice che espone un segmento chiave numerico (`:0:`, `:1:`, &hellip; `:{n}:`) supporta l'associazione di matrici a una matrice di classi POCO.</span><span class="sxs-lookup"><span data-stu-id="28a23-553">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="28a23-554">L'associazione viene fornita per convenzione.</span><span class="sxs-lookup"><span data-stu-id="28a23-554">Binding is provided by convention.</span></span> <span data-ttu-id="28a23-555">I provider di configurazione personalizzati non devono implementare l'associazione di matrici.</span><span class="sxs-lookup"><span data-stu-id="28a23-555">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="28a23-556">**Elaborazione di matrici in memoria**</span><span class="sxs-lookup"><span data-stu-id="28a23-556">**In-memory array processing**</span></span>

<span data-ttu-id="28a23-557">Prendere in considerazione le chiavi di configurazione e i valori indicati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="28a23-557">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="28a23-558">Chiave</span><span class="sxs-lookup"><span data-stu-id="28a23-558">Key</span></span>     | <span data-ttu-id="28a23-559">Valore</span><span class="sxs-lookup"><span data-stu-id="28a23-559">Value</span></span>  |
| :-----: | :----: |
| <span data-ttu-id="28a23-560">array:0</span><span class="sxs-lookup"><span data-stu-id="28a23-560">array:0</span></span> | <span data-ttu-id="28a23-561">value0</span><span class="sxs-lookup"><span data-stu-id="28a23-561">value0</span></span> |
| <span data-ttu-id="28a23-562">array:1</span><span class="sxs-lookup"><span data-stu-id="28a23-562">array:1</span></span> | <span data-ttu-id="28a23-563">value1</span><span class="sxs-lookup"><span data-stu-id="28a23-563">value1</span></span> |
| <span data-ttu-id="28a23-564">array:2</span><span class="sxs-lookup"><span data-stu-id="28a23-564">array:2</span></span> | <span data-ttu-id="28a23-565">value2</span><span class="sxs-lookup"><span data-stu-id="28a23-565">value2</span></span> |
| <span data-ttu-id="28a23-566">array:4</span><span class="sxs-lookup"><span data-stu-id="28a23-566">array:4</span></span> | <span data-ttu-id="28a23-567">value4</span><span class="sxs-lookup"><span data-stu-id="28a23-567">value4</span></span> |
| <span data-ttu-id="28a23-568">array:5</span><span class="sxs-lookup"><span data-stu-id="28a23-568">array:5</span></span> | <span data-ttu-id="28a23-569">value5</span><span class="sxs-lookup"><span data-stu-id="28a23-569">value5</span></span> |

<span data-ttu-id="28a23-570">Queste chiavi e i valori vengono caricati nell'app di esempio usando il provider di configurazione della memoria:</span><span class="sxs-lookup"><span data-stu-id="28a23-570">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="28a23-571">La matrice ignora un valore per l'indice &num;3.</span><span class="sxs-lookup"><span data-stu-id="28a23-571">The array skips a value for index &num;3.</span></span> <span data-ttu-id="28a23-572">Lo strumento di associazione di configurazione non è in grado di associare valori null o di creare voci null negli oggetti associati, situazione che diventerà chiara tra poco quando viene illustrato il risultato dell'associazione di questa matrice a un oggetto.</span><span class="sxs-lookup"><span data-stu-id="28a23-572">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="28a23-573">Nell'app di esempio, è disponibile una classe POCO per i dati di configurazione associati:</span><span class="sxs-lookup"><span data-stu-id="28a23-573">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="28a23-574">I dati di configurazione sono associati all'oggetto:</span><span class="sxs-lookup"><span data-stu-id="28a23-574">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="28a23-575">È possibile usare anche la sintassi [ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*), che produce codice più compatto:</span><span class="sxs-lookup"><span data-stu-id="28a23-575">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="28a23-576">L'oggetto associato, un'istanza di `ArrayExample`, riceve i dati di matrice dalla configurazione.</span><span class="sxs-lookup"><span data-stu-id="28a23-576">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="28a23-577">Indice di `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="28a23-577">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="28a23-578">Valore di `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="28a23-578">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="28a23-579">0</span><span class="sxs-lookup"><span data-stu-id="28a23-579">0</span></span>                             | <span data-ttu-id="28a23-580">value0</span><span class="sxs-lookup"><span data-stu-id="28a23-580">value0</span></span>                        |
| <span data-ttu-id="28a23-581">1</span><span class="sxs-lookup"><span data-stu-id="28a23-581">1</span></span>                             | <span data-ttu-id="28a23-582">value1</span><span class="sxs-lookup"><span data-stu-id="28a23-582">value1</span></span>                        |
| <span data-ttu-id="28a23-583">2</span><span class="sxs-lookup"><span data-stu-id="28a23-583">2</span></span>                             | <span data-ttu-id="28a23-584">value2</span><span class="sxs-lookup"><span data-stu-id="28a23-584">value2</span></span>                        |
| <span data-ttu-id="28a23-585">3</span><span class="sxs-lookup"><span data-stu-id="28a23-585">3</span></span>                             | <span data-ttu-id="28a23-586">value4</span><span class="sxs-lookup"><span data-stu-id="28a23-586">value4</span></span>                        |
| <span data-ttu-id="28a23-587">4</span><span class="sxs-lookup"><span data-stu-id="28a23-587">4</span></span>                             | <span data-ttu-id="28a23-588">value5</span><span class="sxs-lookup"><span data-stu-id="28a23-588">value5</span></span>                        |

<span data-ttu-id="28a23-589">L'indice &num;3 nell'oggetto associato contiene i dati di configurazione per la chiave di configurazione `array:4` e il relativo valore `value4`.</span><span class="sxs-lookup"><span data-stu-id="28a23-589">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="28a23-590">Quando i dati di configurazione contenenti una matrice vengono associati, gli indici di matrice nelle chiavi di configurazione vengono usati semplicemente per scorrere i dati di configurazione quando si crea l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="28a23-590">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="28a23-591">Un valore null non può essere mantenuto nei dati di configurazione e una voce con valore null non viene creata in un oggetto associato quando una matrice nelle chiavi di configurazione ignora uno o più indici.</span><span class="sxs-lookup"><span data-stu-id="28a23-591">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="28a23-592">L'elemento di configurazione mancante per l'indice &num;3 può essere fornito prima dell'associazione all'istanza `ArrayExamples` da qualsiasi provider di configurazione che produce la coppia chiave-valore corretta nella configurazione.</span><span class="sxs-lookup"><span data-stu-id="28a23-592">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExamples` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="28a23-593">Se il codice di esempio include un provider di configurazione JSON aggiuntivo con la coppia chiave-valore mancante, `ArrayExamples.Entries` corrisponde alla matrice di configurazione completa:</span><span class="sxs-lookup"><span data-stu-id="28a23-593">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExamples.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="28a23-594">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="28a23-594">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="28a23-595">In `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="28a23-595">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="28a23-596">Nel costruttore `Startup`:</span><span class="sxs-lookup"><span data-stu-id="28a23-596">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="28a23-597">La coppia chiave-valore mostrata nella tabella viene caricata nella configurazione.</span><span class="sxs-lookup"><span data-stu-id="28a23-597">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="28a23-598">Chiave</span><span class="sxs-lookup"><span data-stu-id="28a23-598">Key</span></span>             | <span data-ttu-id="28a23-599">Valore</span><span class="sxs-lookup"><span data-stu-id="28a23-599">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="28a23-600">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="28a23-600">array:entries:3</span></span> | <span data-ttu-id="28a23-601">value3</span><span class="sxs-lookup"><span data-stu-id="28a23-601">value3</span></span> |

<span data-ttu-id="28a23-602">Se l'istanza della classe `ArrayExamples` viene associata dopo che il provider di configurazione JSON include la voce per l'indice &num;3, la matrice `ArrayExamples.Entries` include il valore.</span><span class="sxs-lookup"><span data-stu-id="28a23-602">If the `ArrayExamples` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExamples.Entries` array includes the value.</span></span>

| <span data-ttu-id="28a23-603">Indice di `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="28a23-603">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="28a23-604">Valore di `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="28a23-604">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="28a23-605">0</span><span class="sxs-lookup"><span data-stu-id="28a23-605">0</span></span>                             | <span data-ttu-id="28a23-606">value0</span><span class="sxs-lookup"><span data-stu-id="28a23-606">value0</span></span>                        |
| <span data-ttu-id="28a23-607">1</span><span class="sxs-lookup"><span data-stu-id="28a23-607">1</span></span>                             | <span data-ttu-id="28a23-608">value1</span><span class="sxs-lookup"><span data-stu-id="28a23-608">value1</span></span>                        |
| <span data-ttu-id="28a23-609">2</span><span class="sxs-lookup"><span data-stu-id="28a23-609">2</span></span>                             | <span data-ttu-id="28a23-610">value2</span><span class="sxs-lookup"><span data-stu-id="28a23-610">value2</span></span>                        |
| <span data-ttu-id="28a23-611">3</span><span class="sxs-lookup"><span data-stu-id="28a23-611">3</span></span>                             | <span data-ttu-id="28a23-612">value3</span><span class="sxs-lookup"><span data-stu-id="28a23-612">value3</span></span>                        |
| <span data-ttu-id="28a23-613">4</span><span class="sxs-lookup"><span data-stu-id="28a23-613">4</span></span>                             | <span data-ttu-id="28a23-614">value4</span><span class="sxs-lookup"><span data-stu-id="28a23-614">value4</span></span>                        |
| <span data-ttu-id="28a23-615">5</span><span class="sxs-lookup"><span data-stu-id="28a23-615">5</span></span>                             | <span data-ttu-id="28a23-616">value5</span><span class="sxs-lookup"><span data-stu-id="28a23-616">value5</span></span>                        |

<span data-ttu-id="28a23-617">**Elaborazione di matrice JSON**</span><span class="sxs-lookup"><span data-stu-id="28a23-617">**JSON array processing**</span></span>

<span data-ttu-id="28a23-618">Se un file JSON contiene una matrice, vengono create chiavi di configurazione per gli elementi della matrice con un indice di sezione in base zero.</span><span class="sxs-lookup"><span data-stu-id="28a23-618">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="28a23-619">Nel file di configurazione seguente, `subsection` è una matrice:</span><span class="sxs-lookup"><span data-stu-id="28a23-619">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="28a23-620">Il provider di configurazione JSON legge i dati di configurazione nelle coppie chiave-valore seguenti:</span><span class="sxs-lookup"><span data-stu-id="28a23-620">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="28a23-621">Chiave</span><span class="sxs-lookup"><span data-stu-id="28a23-621">Key</span></span>                     | <span data-ttu-id="28a23-622">Valore</span><span class="sxs-lookup"><span data-stu-id="28a23-622">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="28a23-623">json_array:key</span><span class="sxs-lookup"><span data-stu-id="28a23-623">json_array:key</span></span>          | <span data-ttu-id="28a23-624">valueA</span><span class="sxs-lookup"><span data-stu-id="28a23-624">valueA</span></span> |
| <span data-ttu-id="28a23-625">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="28a23-625">json_array:subsection:0</span></span> | <span data-ttu-id="28a23-626">valueB</span><span class="sxs-lookup"><span data-stu-id="28a23-626">valueB</span></span> |
| <span data-ttu-id="28a23-627">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="28a23-627">json_array:subsection:1</span></span> | <span data-ttu-id="28a23-628">valueC</span><span class="sxs-lookup"><span data-stu-id="28a23-628">valueC</span></span> |
| <span data-ttu-id="28a23-629">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="28a23-629">json_array:subsection:2</span></span> | <span data-ttu-id="28a23-630">valueD</span><span class="sxs-lookup"><span data-stu-id="28a23-630">valueD</span></span> |

<span data-ttu-id="28a23-631">Nell'app di esempio, la classe POCO seguente è disponibile per associare le coppie chiave-valore della configurazione:</span><span class="sxs-lookup"><span data-stu-id="28a23-631">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="28a23-632">Dopo l'associazione, `JsonArrayExample.Key` contiene il valore `valueA`.</span><span class="sxs-lookup"><span data-stu-id="28a23-632">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="28a23-633">I valori di sottosezione vengono archiviati nella proprietà matrice POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="28a23-633">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="28a23-634">Indice di `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="28a23-634">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="28a23-635">Valore di `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="28a23-635">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="28a23-636">0</span><span class="sxs-lookup"><span data-stu-id="28a23-636">0</span></span>                                   | <span data-ttu-id="28a23-637">valueB</span><span class="sxs-lookup"><span data-stu-id="28a23-637">valueB</span></span>                              |
| <span data-ttu-id="28a23-638">1</span><span class="sxs-lookup"><span data-stu-id="28a23-638">1</span></span>                                   | <span data-ttu-id="28a23-639">valueC</span><span class="sxs-lookup"><span data-stu-id="28a23-639">valueC</span></span>                              |
| <span data-ttu-id="28a23-640">2</span><span class="sxs-lookup"><span data-stu-id="28a23-640">2</span></span>                                   | <span data-ttu-id="28a23-641">valueD</span><span class="sxs-lookup"><span data-stu-id="28a23-641">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="28a23-642">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="28a23-642">Custom configuration provider</span></span>

<span data-ttu-id="28a23-643">L'app di esempio dimostra come creare un provider di configurazione di base che legge le coppie chiave-valore di configurazione da un database usando [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="28a23-643">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="28a23-644">Il provider ha le caratteristiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="28a23-644">The provider has the following characteristics:</span></span>

* <span data-ttu-id="28a23-645">Il database in memoria di Entity Framework viene usato a scopo dimostrativo.</span><span class="sxs-lookup"><span data-stu-id="28a23-645">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="28a23-646">Per usare un database che richiede una stringa di connessione, implementare un `ConfigurationBuilder` secondario per fornire la stringa di connessione da un altro provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="28a23-646">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="28a23-647">Il provider legge una tabella di database in una configurazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="28a23-647">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="28a23-648">Il provider non esegue una query sul database per ogni chiave.</span><span class="sxs-lookup"><span data-stu-id="28a23-648">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="28a23-649">Il ricaricamento in caso di modifica non è implementato, quindi l'aggiornamento del database dopo l'avvio dell'app non ha alcun effetto sulla configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="28a23-649">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="28a23-650">Definire un'entità `EFConfigurationValue` per l'archiviazione dei valori di configurazione nel database.</span><span class="sxs-lookup"><span data-stu-id="28a23-650">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="28a23-651">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="28a23-651">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="28a23-652">Aggiungere `EFConfigurationContext` per archiviare i valori configurati e accedervi.</span><span class="sxs-lookup"><span data-stu-id="28a23-652">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="28a23-653">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="28a23-653">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="28a23-654">Creare una classe che implementi <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="28a23-654">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="28a23-655">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="28a23-655">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="28a23-656">Creare il provider di configurazione personalizzato ereditando da <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="28a23-656">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="28a23-657">Il provider di configurazione inizializza il database quando è vuoto.</span><span class="sxs-lookup"><span data-stu-id="28a23-657">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="28a23-658">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="28a23-658">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="28a23-659">Un metodo di estensione `AddEFConfiguration` consente di aggiungere l'origine di configurazione a un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="28a23-659">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="28a23-660">*EFConfigurationProvider/EFConfigurationExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="28a23-660">*EFConfigurationProvider/EFConfigurationExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="28a23-661">L'esempio di codice seguente mostra come usare il `EFConfigurationProvider` personalizzato in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="28a23-661">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="28a23-662">Accedere alla configurazione durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="28a23-662">Access configuration during startup</span></span>

<span data-ttu-id="28a23-663">Inserire `IConfiguration` nel costruttore `Startup` per accedere ai valori di configurazione in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="28a23-663">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="28a23-664">Per accedere alla configurazione in `Startup.Configure`, inserire `IConfiguration` direttamente nel metodo o usare l'istanza dal costruttore:</span><span class="sxs-lookup"><span data-stu-id="28a23-664">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="28a23-665">Per un esempio di accesso alla configurazione usando metodi di servizio di avvio, vedere [Avvio dell'applicazione: Metodi pratici](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="28a23-665">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="28a23-666">Accedere alla configurazione in una pagina Razor Pages o in una visualizzazione MVC</span><span class="sxs-lookup"><span data-stu-id="28a23-666">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="28a23-667">Per accedere alle impostazioni di configurazione in una pagina Razor Pages o in una visualizzazione MVC, aggiungere un [direttiva using](xref:mvc/views/razor#using) ([Informazioni di riferimento su C#: direttiva using](/dotnet/csharp/language-reference/keywords/using-directive)) per lo [spazio dei nomi Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) e inserire <xref:Microsoft.Extensions.Configuration.IConfiguration> nella pagina o nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="28a23-667">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="28a23-668">In una pagina Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="28a23-668">In a Razor Pages page:</span></span>

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

<span data-ttu-id="28a23-669">In una visualizzazione MVC:</span><span class="sxs-lookup"><span data-stu-id="28a23-669">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="28a23-670">Aggiungere la configurazione da un assembly esterno</span><span class="sxs-lookup"><span data-stu-id="28a23-670">Add configuration from an external assembly</span></span>

<span data-ttu-id="28a23-671">Un'implementazione <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> consente l'aggiunta di miglioramenti a un'app all'avvio, da un assembly esterno alla classe `Startup` dell'app.</span><span class="sxs-lookup"><span data-stu-id="28a23-671">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="28a23-672">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="28a23-672">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="28a23-673">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="28a23-673">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* <span data-ttu-id="28a23-674">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/) (Configurazione Microsoft in dettaglio)</span><span class="sxs-lookup"><span data-stu-id="28a23-674">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)</span></span>
