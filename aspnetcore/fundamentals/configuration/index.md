---
title: Configurazione in ASP.NET Core
author: guardrex
description: Informazioni su come usare l'API di configurazione per configurare un'app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 6f0378ffc4f9a1efa95c8f70d70e7799abef130b
ms.sourcegitcommit: 1872d2e6f299093c78a6795a486929ffb0bbffff
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/11/2018
ms.locfileid: "53216898"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="db393-103">Configurazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="db393-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="db393-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="db393-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="db393-105">La configurazione delle app in ASP.NET Core si basa su coppie chiave-valore stabilite dai *provider di configurazione*.</span><span class="sxs-lookup"><span data-stu-id="db393-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="db393-106">I provider di configurazione leggono i dati di configurazione in coppie chiave-valore da un'ampia gamma di origini di configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="db393-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="db393-107">Azure Key Vault</span></span>
* <span data-ttu-id="db393-108">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="db393-108">Command-line arguments</span></span>
* <span data-ttu-id="db393-109">Provider personalizzati (installati o creati)</span><span class="sxs-lookup"><span data-stu-id="db393-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="db393-110">File della directory</span><span class="sxs-lookup"><span data-stu-id="db393-110">Directory files</span></span>
* <span data-ttu-id="db393-111">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="db393-111">Environment variables</span></span>
* <span data-ttu-id="db393-112">Oggetti .NET in memoria</span><span class="sxs-lookup"><span data-stu-id="db393-112">In-memory .NET objects</span></span>
* <span data-ttu-id="db393-113">File di impostazioni</span><span class="sxs-lookup"><span data-stu-id="db393-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="db393-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="db393-114">Azure Key Vault</span></span>
* <span data-ttu-id="db393-115">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="db393-115">Command-line arguments</span></span>
* <span data-ttu-id="db393-116">Provider personalizzati (installati o creati)</span><span class="sxs-lookup"><span data-stu-id="db393-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="db393-117">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="db393-117">Environment variables</span></span>
* <span data-ttu-id="db393-118">Oggetti .NET in memoria</span><span class="sxs-lookup"><span data-stu-id="db393-118">In-memory .NET objects</span></span>
* <span data-ttu-id="db393-119">File di impostazioni</span><span class="sxs-lookup"><span data-stu-id="db393-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="db393-120">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="db393-120">Command-line arguments</span></span>
* <span data-ttu-id="db393-121">Provider personalizzati (installati o creati)</span><span class="sxs-lookup"><span data-stu-id="db393-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="db393-122">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="db393-122">Environment variables</span></span>
* <span data-ttu-id="db393-123">Oggetti .NET in memoria</span><span class="sxs-lookup"><span data-stu-id="db393-123">In-memory .NET objects</span></span>
* <span data-ttu-id="db393-124">File di impostazioni</span><span class="sxs-lookup"><span data-stu-id="db393-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="db393-125">Il *modello di opzioni* è un'estensione dei concetti di configurazione descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="db393-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="db393-126">Le opzioni usano le classi per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="db393-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="db393-127">Per altre informazioni sull'uso del modello di opzioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="db393-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="db393-128">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="db393-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="db393-129">Gli esempi in questo argomento si basano su:</span><span class="sxs-lookup"><span data-stu-id="db393-129">The examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="db393-130">Impostazione del percorso base dell'app con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="db393-130">Setting the base path of the app with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="db393-131">`SetBasePath` viene resa disponibile per un'app facendo riferimento al pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/).</span><span class="sxs-lookup"><span data-stu-id="db393-131">`SetBasePath` is made available to an app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="db393-132">Risoluzione di sezioni dei file di configurazione con <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span><span class="sxs-lookup"><span data-stu-id="db393-132">Resolving sections of configuration files with <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span></span> <span data-ttu-id="db393-133">`GetSection` viene resa disponibile per un'app facendo riferimento al pacchetto [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/).</span><span class="sxs-lookup"><span data-stu-id="db393-133">`GetSection` is made available to an app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="db393-134">Associazione della configurazione a classi .NET con <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> e [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span><span class="sxs-lookup"><span data-stu-id="db393-134">Binding configuration to .NET classes with <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> and [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span></span> <span data-ttu-id="db393-135">`Bind` e `Get<T>` vengono rese disponibili per un'app facendo riferimento al pacchetto [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/).</span><span class="sxs-lookup"><span data-stu-id="db393-135">`Bind` and `Get<T>` are made available to an app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span> <span data-ttu-id="db393-136">`Get<T>` è disponibile in ASP.NET Core 1.1 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="db393-136">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="db393-137">Questi tre pacchetti sono inclusi nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="db393-137">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="db393-138">Questi tre pacchetti sono inclusi nel [metapacchetto Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="db393-138">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="db393-139">Host e configurazione delle app</span><span class="sxs-lookup"><span data-stu-id="db393-139">Host vs. app configuration</span></span>

<span data-ttu-id="db393-140">Prima che l'app venga configurata e avviata, viene configurato e avviato un *host*.</span><span class="sxs-lookup"><span data-stu-id="db393-140">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="db393-141">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="db393-141">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="db393-142">Sia l'app che l'host vengono configurati tramite i provider di configurazione descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="db393-142">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="db393-143">Le coppie chiave-valore di configurazione dell'host diventano parte della configurazione globale dell'app.</span><span class="sxs-lookup"><span data-stu-id="db393-143">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="db393-144">Per altre informazioni su come vengono usati i provider di configurazione quando viene creato l'host e sugli effetti delle origini di configurazione sull'host di configurazione, vedere <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="db393-144">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="security"></a><span data-ttu-id="db393-145">Sicurezza</span><span class="sxs-lookup"><span data-stu-id="db393-145">Security</span></span>

<span data-ttu-id="db393-146">Adottare le procedure consigliate seguenti:</span><span class="sxs-lookup"><span data-stu-id="db393-146">Adopt the following best practices:</span></span>

* <span data-ttu-id="db393-147">Non archiviare mai la password o altri dati sensibili nel codice del provider di configurazione o in file di configurazione di testo normale.</span><span class="sxs-lookup"><span data-stu-id="db393-147">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="db393-148">Non usare i segreti di produzione in ambienti di sviluppo o di test.</span><span class="sxs-lookup"><span data-stu-id="db393-148">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="db393-149">Specificare i segreti all'esterno del progetto in modo che non possano essere inavvertitamente inviati a un repository del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="db393-149">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="db393-150">Altre informazioni su [come usare più ambienti](xref:fundamentals/environments) e la gestione dell'[archiviazione sicura dei segreti delle app durante lo sviluppo con Secret Manager](xref:security/app-secrets) (include consigli sull'uso delle variabili di ambiente per l'archiviazione di dati sensibili).</span><span class="sxs-lookup"><span data-stu-id="db393-150">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="db393-151">Secret Manager usa il provider di configurazione dei file per archiviare i segreti utente in un file JSON nel sistema locale.</span><span class="sxs-lookup"><span data-stu-id="db393-151">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="db393-152">Il provider di configurazione dei file è descritto più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="db393-152">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="db393-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) è una delle opzioni per l'archiviazione sicura dei segreti delle app.</span><span class="sxs-lookup"><span data-stu-id="db393-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="db393-154">Per ulteriori informazioni, vedere <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="db393-154">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="db393-155">Dati di configurazione gerarchici</span><span class="sxs-lookup"><span data-stu-id="db393-155">Hierarchical configuration data</span></span>

<span data-ttu-id="db393-156">L'API di configurazione è in grado di mantenere i dati di configurazione gerarchici rendendoli flat grazie all'uso di un delimitatore nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="db393-156">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="db393-157">Nel file JSON seguente, esistono quattro chiavi in una gerarchia strutturata di due sezioni:</span><span class="sxs-lookup"><span data-stu-id="db393-157">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="db393-158">Quando il file viene letto nella configurazione, vengono create chiavi univoche per mantenere la struttura di dati gerarchici originale dell'origine di configurazione.</span><span class="sxs-lookup"><span data-stu-id="db393-158">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="db393-159">Le sezioni e le chiavi vengono rese flat usando due punti (`:`) per mantenere la struttura originale:</span><span class="sxs-lookup"><span data-stu-id="db393-159">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="db393-160">section0:key0</span><span class="sxs-lookup"><span data-stu-id="db393-160">section0:key0</span></span>
* <span data-ttu-id="db393-161">section0:key1</span><span class="sxs-lookup"><span data-stu-id="db393-161">section0:key1</span></span>
* <span data-ttu-id="db393-162">section1:key0</span><span class="sxs-lookup"><span data-stu-id="db393-162">section1:key0</span></span>
* <span data-ttu-id="db393-163">section1:key1</span><span class="sxs-lookup"><span data-stu-id="db393-163">section1:key1</span></span>

<span data-ttu-id="db393-164">I metodi <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> e <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> sono disponibili per isolare le sezioni e gli elementi figlio di una sezione nei dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="db393-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="db393-165">Questi metodi sono descritti più avanti in [GetSection, GetChildren ed Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="db393-165">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="db393-166">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="db393-166">Conventions</span></span>

<span data-ttu-id="db393-167">All'avvio dell'app, le origini di configurazione vengono lette nell'ordine con cui sono specificati i rispettivi provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="db393-167">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="db393-168">I provider di configurazione dei file supportano il ricaricamento della configurazione quando un file di impostazioni sottostante viene modificato dopo l'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="db393-168">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="db393-169">Il provider di configurazione dei file è descritto più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="db393-169">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="db393-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> è disponibile nel contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) dell'app.</span><span class="sxs-lookup"><span data-stu-id="db393-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="db393-171">I provider di configurazione non possono usare l'inserimento delle dipendenze, perché non è disponibile quando vengono configurati dall'host.</span><span class="sxs-lookup"><span data-stu-id="db393-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="db393-172">Le chiavi di configurazione adottano le convenzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="db393-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="db393-173">Per le chiavi non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="db393-173">Keys are case-insensitive.</span></span> <span data-ttu-id="db393-174">Ad esempio, `ConnectionString` e `connectionstring` vengono considerate chiavi equivalenti.</span><span class="sxs-lookup"><span data-stu-id="db393-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="db393-175">Se un valore per la stessa chiave viene impostato dallo stesso provider di configurazione o da provider diversi, viene usato l'ultimo valore impostato per la chiave.</span><span class="sxs-lookup"><span data-stu-id="db393-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="db393-176">Chiavi gerarchiche</span><span class="sxs-lookup"><span data-stu-id="db393-176">Hierarchical keys</span></span>
  * <span data-ttu-id="db393-177">Nell'ambito dell'API di configurazione, il separatore due punti (`:`) funziona in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="db393-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="db393-178">Nelle variabili di ambiente, un separatore due punti potrebbe non funzionare in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="db393-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="db393-179">Il doppio carattere di sottolineatura (`__`) è supportato da tutte le piattaforme e viene convertito in due punti.</span><span class="sxs-lookup"><span data-stu-id="db393-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="db393-180">In Azure Key Vault, le chiavi gerarchiche usano `--` (due trattini) come separatore.</span><span class="sxs-lookup"><span data-stu-id="db393-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="db393-181">È necessario fornire il codice per sostituire i trattini con due punti quando i segreti vengono caricati nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="db393-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="db393-182">Il <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supporta l'associazione di matrici agli oggetti usando gli indici delle matrici nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="db393-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="db393-183">L'associazione di matrici è descritta nella sezione [Associare una matrice a una classe](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="db393-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="db393-184">I valori di configurazione adottano le convenzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="db393-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="db393-185">I valori sono stringhe.</span><span class="sxs-lookup"><span data-stu-id="db393-185">Values are strings.</span></span>
* <span data-ttu-id="db393-186">I valori null non possono essere archiviati nella configurazione o associati a oggetti.</span><span class="sxs-lookup"><span data-stu-id="db393-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="db393-187">Provider</span><span class="sxs-lookup"><span data-stu-id="db393-187">Providers</span></span>

<span data-ttu-id="db393-188">La tabella seguente mostra i provider di configurazione disponibili per le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="db393-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="db393-189">Provider</span><span class="sxs-lookup"><span data-stu-id="db393-189">Provider</span></span> | <span data-ttu-id="db393-190">Fornisce la configurazione da&hellip;</span><span class="sxs-lookup"><span data-stu-id="db393-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="db393-191">[Provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="db393-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="db393-192">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="db393-192">Azure Key Vault</span></span> |
| [<span data-ttu-id="db393-193">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="db393-193">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="db393-194">Parametri della riga di comando</span><span class="sxs-lookup"><span data-stu-id="db393-194">Command-line parameters</span></span> |
| [<span data-ttu-id="db393-195">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="db393-195">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="db393-196">Origine personalizzata</span><span class="sxs-lookup"><span data-stu-id="db393-196">Custom source</span></span> |
| [<span data-ttu-id="db393-197">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="db393-197">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="db393-198">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="db393-198">Environment variables</span></span> |
| [<span data-ttu-id="db393-199">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="db393-199">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="db393-200">File (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="db393-200">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="db393-201">Provider di configurazione chiave-per-file</span><span class="sxs-lookup"><span data-stu-id="db393-201">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="db393-202">File della directory</span><span class="sxs-lookup"><span data-stu-id="db393-202">Directory files</span></span> |
| [<span data-ttu-id="db393-203">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="db393-203">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="db393-204">Raccolte in memoria</span><span class="sxs-lookup"><span data-stu-id="db393-204">In-memory collections</span></span> |
| <span data-ttu-id="db393-205">[Segreti utente (Secret Manager)](xref:security/app-secrets) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="db393-205">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="db393-206">File nella directory dei profili utente</span><span class="sxs-lookup"><span data-stu-id="db393-206">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="db393-207">Provider</span><span class="sxs-lookup"><span data-stu-id="db393-207">Provider</span></span> | <span data-ttu-id="db393-208">Fornisce la configurazione da&hellip;</span><span class="sxs-lookup"><span data-stu-id="db393-208">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="db393-209">[Provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="db393-209">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="db393-210">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="db393-210">Azure Key Vault</span></span> |
| [<span data-ttu-id="db393-211">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="db393-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="db393-212">Parametri della riga di comando</span><span class="sxs-lookup"><span data-stu-id="db393-212">Command-line parameters</span></span> |
| [<span data-ttu-id="db393-213">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="db393-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="db393-214">Origine personalizzata</span><span class="sxs-lookup"><span data-stu-id="db393-214">Custom source</span></span> |
| [<span data-ttu-id="db393-215">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="db393-215">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="db393-216">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="db393-216">Environment variables</span></span> |
| [<span data-ttu-id="db393-217">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="db393-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="db393-218">File (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="db393-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="db393-219">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="db393-219">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="db393-220">Raccolte in memoria</span><span class="sxs-lookup"><span data-stu-id="db393-220">In-memory collections</span></span> |
| <span data-ttu-id="db393-221">[Segreti utente (Secret Manager)](xref:security/app-secrets) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="db393-221">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="db393-222">File nella directory dei profili utente</span><span class="sxs-lookup"><span data-stu-id="db393-222">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="db393-223">Provider</span><span class="sxs-lookup"><span data-stu-id="db393-223">Provider</span></span> | <span data-ttu-id="db393-224">Fornisce la configurazione da&hellip;</span><span class="sxs-lookup"><span data-stu-id="db393-224">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="db393-225">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="db393-225">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="db393-226">Parametri della riga di comando</span><span class="sxs-lookup"><span data-stu-id="db393-226">Command-line parameters</span></span> |
| [<span data-ttu-id="db393-227">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="db393-227">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="db393-228">Origine personalizzata</span><span class="sxs-lookup"><span data-stu-id="db393-228">Custom source</span></span> |
| [<span data-ttu-id="db393-229">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="db393-229">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="db393-230">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="db393-230">Environment variables</span></span> |
| [<span data-ttu-id="db393-231">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="db393-231">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="db393-232">File (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="db393-232">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="db393-233">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="db393-233">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="db393-234">Raccolte in memoria</span><span class="sxs-lookup"><span data-stu-id="db393-234">In-memory collections</span></span> |
| <span data-ttu-id="db393-235">[Segreti utente (Secret Manager)](xref:security/app-secrets) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="db393-235">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="db393-236">File nella directory dei profili utente</span><span class="sxs-lookup"><span data-stu-id="db393-236">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="db393-237">Le origini di configurazione vengono lette nell'ordine in cui vengono specificati i rispetti provider di configurazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="db393-237">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="db393-238">I provider di configurazione descritti in questo argomento vengono presentati in ordine alfabetico e non nell'ordine in cui potrebbe disporli il codice.</span><span class="sxs-lookup"><span data-stu-id="db393-238">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="db393-239">Ordinare i provider di configurazione nel codice in base alle specifiche priorità per le origini di configurazione sottostanti.</span><span class="sxs-lookup"><span data-stu-id="db393-239">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="db393-240">Una sequenza tipica di provider di configurazione è:</span><span class="sxs-lookup"><span data-stu-id="db393-240">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="db393-241">File (*appsettings.json*, *appsettings.{Environment}.json*, dove `{Environment}` è l'ambiente di hosting corrente dell'app)</span><span class="sxs-lookup"><span data-stu-id="db393-241">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="db393-242">Insieme di credenziali delle chiavi di Azure</span><span class="sxs-lookup"><span data-stu-id="db393-242">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="db393-243">[Segreti utente (Secret Manager)](xref:security/app-secrets) (sono nell'ambiente Development)</span><span class="sxs-lookup"><span data-stu-id="db393-243">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="db393-244">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="db393-244">Environment variables</span></span>
1. <span data-ttu-id="db393-245">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="db393-245">Command-line arguments</span></span>

<span data-ttu-id="db393-246">È pratica comune posizionare il provider di configurazione della riga di comando per ultimo in una serie di provider per consentire agli argomenti della riga di comando di sostituire la configurazione impostata da altri provider.</span><span class="sxs-lookup"><span data-stu-id="db393-246">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="db393-247">Questa sequenza di provider viene applicata quando si inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="db393-247">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="db393-248">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="db393-248">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="db393-249">È possibile creare questa sequenza di provider per l'app (non l'host) con un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> e una chiamata al relativo metodo <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="db393-249">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

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

<span data-ttu-id="db393-250">Nell'esempio precedente, il nome dell'ambiente (`env.EnvironmentName`) e il nome di assembly dell'app (`env.ApplicationName`) vengono forniti da <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="db393-250">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="db393-251">Per ulteriori informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="db393-251">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="configureappconfiguration"></a><span data-ttu-id="db393-252">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="db393-252">ConfigureAppConfiguration</span></span>

<span data-ttu-id="db393-253">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host Web per specificare i provider di configurazione dell'app, oltre a quelli aggiunti automaticamente da <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="db393-253">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the Web Host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="db393-254">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="db393-254">Command-line Configuration Provider</span></span>

<span data-ttu-id="db393-255"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> carica la configurazione da coppie chiave-valore di argomenti della riga di comando in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="db393-255">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="db393-256">Per attivare la configurazione della riga di comando, metodo di estensione <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> viene chiamato su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="db393-256">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="db393-257">`AddCommandLine` viene chiamato automaticamente quando si Inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="db393-257">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="db393-258">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="db393-258">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="db393-259">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="db393-259">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="db393-260">Una configurazione facoltativa da *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="db393-260">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="db393-261">[Segreti utente (Secret Manager)](xref:security/app-secrets) (nell'ambiente Development).</span><span class="sxs-lookup"><span data-stu-id="db393-261">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="db393-262">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="db393-262">Environment variables.</span></span>

<span data-ttu-id="db393-263">`CreateDefaultBuilder` aggiunge il provider di configurazione della riga di comando per ultimo.</span><span class="sxs-lookup"><span data-stu-id="db393-263">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="db393-264">Gli argomenti della riga di comando passati in fase di esecuzione sostituiscono la configurazione impostata dagli altri provider.</span><span class="sxs-lookup"><span data-stu-id="db393-264">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="db393-265">`CreateDefaultBuilder` opera durante la creazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="db393-265">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="db393-266">Pertanto, la configurazione della riga di comando attivata da `CreateDefaultBuilder` può influire sul modo in cui l'host viene configurato.</span><span class="sxs-lookup"><span data-stu-id="db393-266">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="db393-267">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="db393-267">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="db393-268">`AddCommandLine` è già stato chiamato da `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="db393-268">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="db393-269">Se è necessario specificare la configurazione dell'app ed è ancora possibile eseguire l'override della configurazione con argomenti della riga di comando, chiamare i provider aggiuntivi dell'app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> e chiamare infine `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="db393-269">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="db393-270">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-270">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="db393-271">Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="db393-271">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="db393-272">`AddCommandLine` è già stato chiamato da `CreateDefaultBuilder` quando è stato chiamato `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="db393-272">`AddCommandLine` has already been called by `CreateDefaultBuilder` when `UseConfiguration` is called.</span></span> <span data-ttu-id="db393-273">Se è necessario specificare la configurazione dell'app ed è ancora possibile eseguire l'override della configurazione con argomenti della riga di comando, chiamare i provider aggiuntivi dell'app in un `ConfigurationBuilder` e chiamare infine `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="db393-273">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers on a `ConfigurationBuilder` and call `AddCommandLine` last.</span></span>

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
            // Call other providers here and call AddCommandLine last.
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="db393-274">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-274">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="db393-275">Per attivare la configurazione della riga di comando, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="db393-275">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="db393-276">Chiamare il provider per ultimo per consentire agli argomenti della riga di comando passati in fase di esecuzione di sostituire la configurazione impostata da altri provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="db393-276">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="db393-277">Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="db393-277">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

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

<span data-ttu-id="db393-278">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="db393-278">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="db393-279">L'app di esempio 2.x consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include una chiamata a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="db393-279">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="db393-280">L'app di esempio 1.x chiama <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> per un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="db393-280">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="db393-281">Aprire un prompt dei comandi nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="db393-281">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="db393-282">Fornire un argomento della riga di comando per il comando `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="db393-282">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="db393-283">Dopo che l'app è in esecuzione, aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="db393-283">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="db393-284">Notare che l'output contiene la coppia chiave-valore per l'argomento della riga di comando di configurazione fornito per `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="db393-284">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="db393-285">Argomenti</span><span class="sxs-lookup"><span data-stu-id="db393-285">Arguments</span></span>

<span data-ttu-id="db393-286">Il valore deve seguire un segno di uguale (`=`) o la chiave deve avere un prefisso (`--` o `/`) quando il valore segue uno spazio.</span><span class="sxs-lookup"><span data-stu-id="db393-286">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="db393-287">Il valore può essere null se viene usato un segno di uguale (ad esempio, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="db393-287">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="db393-288">Prefisso chiave</span><span class="sxs-lookup"><span data-stu-id="db393-288">Key prefix</span></span>               | <span data-ttu-id="db393-289">Esempio</span><span class="sxs-lookup"><span data-stu-id="db393-289">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="db393-290">Nessun prefisso</span><span class="sxs-lookup"><span data-stu-id="db393-290">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="db393-291">Due trattini (`--`)</span><span class="sxs-lookup"><span data-stu-id="db393-291">Two dashes (`--`)</span></span>        | <span data-ttu-id="db393-292">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="db393-292">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="db393-293">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="db393-293">Forward slash (`/`)</span></span>      | <span data-ttu-id="db393-294">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="db393-294">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="db393-295">Nello stesso comando, non mischiare coppie chiave-valore di argomenti della riga di comando che usano un segno di uguale con coppie chiave-valore che usano uno spazio.</span><span class="sxs-lookup"><span data-stu-id="db393-295">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="db393-296">Comandi di esempio:</span><span class="sxs-lookup"><span data-stu-id="db393-296">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="db393-297">Mapping di sostituzione</span><span class="sxs-lookup"><span data-stu-id="db393-297">Switch mappings</span></span>

<span data-ttu-id="db393-298">I mapping di sostituzione consentono di usare la logica di sostituzione del nome della chiave.</span><span class="sxs-lookup"><span data-stu-id="db393-298">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="db393-299">Quando si crea manualmente la configurazione con un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, è possibile specificare un dizionario di mapping di sostituzione per il metodo <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="db393-299">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="db393-300">Quando viene utilizzato il dizionario dei mapping di sostituzione, nel dizionario viene controllata la presenza di una chiave corrispondente alla chiave fornita da un argomento della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="db393-300">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="db393-301">Se la chiave della riga di comando viene trovata nel dizionario, il valore del dizionario (la sostituzione della chiave) viene passato nuovamente per impostare la coppia chiave-valore nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="db393-301">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="db393-302">Un mapping di sostituzione è necessario per le chiavi della riga di comando con un trattino singolo (`-`) come prefisso.</span><span class="sxs-lookup"><span data-stu-id="db393-302">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="db393-303">Regole principali del dizionario dei mapping di sostituzione:</span><span class="sxs-lookup"><span data-stu-id="db393-303">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="db393-304">Le sostituzioni devono iniziare con un trattino (`-`) o un trattino doppio (`--`).</span><span class="sxs-lookup"><span data-stu-id="db393-304">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="db393-305">Il dizionario dei mapping di sostituzione non deve contenere chiavi duplicate.</span><span class="sxs-lookup"><span data-stu-id="db393-305">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="db393-306">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="db393-306">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="db393-307">Come illustrato nell'esempio precedente, la chiamata a `CreateDefaultBuilder` non deve passare argomenti quando vengono usati i mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="db393-307">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="db393-308">La chiamata `AddCommandLine` del metodo `CreateDefaultBuilder` non include sostituzioni mappate e non è possibile passare il dizionario dei mapping di sostituzione a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="db393-308">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="db393-309">Se gli argomenti includono una sostituzione mappata e vengono passati a `CreateDefaultBuilder`, l'inizializzazione del rispettivo provider `AddCommandLine` non riesce con <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="db393-309">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="db393-310">La soluzione consiste nel non passare gli argomenti a `CreateDefaultBuilder` consentendo invece al metodo `AddCommandLine` del metodo `ConfigurationBuilder` di elaborare entrambi gli argomenti e il dizionario dei mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="db393-310">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

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

        // Do not pass the args to CreateDefaultBuilder
        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="db393-311">Come illustrato nell'esempio precedente, la chiamata a `CreateDefaultBuilder` non deve passare argomenti quando vengono usati i mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="db393-311">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="db393-312">La chiamata `AddCommandLine` del metodo `CreateDefaultBuilder` non include sostituzioni mappate e non è possibile passare il dizionario dei mapping di sostituzione a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="db393-312">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="db393-313">Se gli argomenti includono una sostituzione mappata e vengono passati a `CreateDefaultBuilder`, l'inizializzazione del rispettivo provider `AddCommandLine` non riesce con <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="db393-313">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="db393-314">La soluzione consiste nel non passare gli argomenti a `CreateDefaultBuilder` consentendo invece al metodo `AddCommandLine` del metodo `ConfigurationBuilder` di elaborare entrambi gli argomenti e il dizionario dei mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="db393-314">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="db393-315">Il dizionario dei mapping di sostituzione creato contiene i dati visualizzati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="db393-315">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="db393-316">Chiave</span><span class="sxs-lookup"><span data-stu-id="db393-316">Key</span></span>       | <span data-ttu-id="db393-317">Value</span><span class="sxs-lookup"><span data-stu-id="db393-317">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="db393-318">Se le chiavi con mapping di sostituzione vengono usate all'avvio dell'app, la configurazione riceve il valore di configurazione per la chiave fornita dal dizionario:</span><span class="sxs-lookup"><span data-stu-id="db393-318">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="db393-319">Dopo aver eseguito il comando precedente, la configurazione contiene i valori mostrati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="db393-319">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="db393-320">Chiave</span><span class="sxs-lookup"><span data-stu-id="db393-320">Key</span></span>               | <span data-ttu-id="db393-321">Value</span><span class="sxs-lookup"><span data-stu-id="db393-321">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="db393-322">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="db393-322">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="db393-323"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> carica la configurazione da coppie chiave-valore di variabili di ambiente in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="db393-323">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="db393-324">Per attivare la configurazione delle variabili di ambiente, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="db393-324">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="db393-325">Quando si utilizzano chiavi gerarchiche in variabili di ambiente, il separatore due punti (`:`) potrebbe non funzionare in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="db393-325">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="db393-326">Il doppio carattere di sottolineatura (`__`) è supportato da tutte le piattaforme e viene sostituito con due punti.</span><span class="sxs-lookup"><span data-stu-id="db393-326">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="db393-327">[Servizio App di Azure](https://azure.microsoft.com/services/app-service/) consente di impostare variabili di ambiente nel portale di Azure che possono sostituire la configurazione delle app tramite il provider di configurazione delle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="db393-327">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="db393-328">Per altre informazioni, vedere [App Azure: Eseguire l'override della configurazione delle app usando il portale di Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="db393-328">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="db393-329">`AddEnvironmentVariables` viene chiamato automaticamente per le variabili di ambiente con prefisso `ASPNETCORE_` quando si inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="db393-329">`AddEnvironmentVariables` is automatically called for environment variables prefixed with `ASPNETCORE_` when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="db393-330">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="db393-330">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="db393-331">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="db393-331">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="db393-332">Configurazione delle app dalle variabili di ambiente senza prefisso chiamando `AddEnvironmentVariables` senza prefisso.</span><span class="sxs-lookup"><span data-stu-id="db393-332">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="db393-333">Una configurazione facoltativa da *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="db393-333">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="db393-334">[Segreti utente (Secret Manager)](xref:security/app-secrets) (nell'ambiente Development).</span><span class="sxs-lookup"><span data-stu-id="db393-334">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="db393-335">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="db393-335">Command-line arguments.</span></span>

<span data-ttu-id="db393-336">Il provider di configurazione delle variabili di ambiente viene chiamato dopo aver stabilito la configurazione dai segreti utente e dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="db393-336">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="db393-337">La chiamata del provider in questa posizione consente alle variabili di ambiente lette in fase di esecuzione di sostituire la configurazione impostata dai segreti utente e dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="db393-337">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="db393-338">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="db393-338">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="db393-339">Se è necessario specificare la configurazione dell'app da variabili di ambiente aggiuntive, chiamare i provider aggiuntivi dell'app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> e chiamare `AddEnvironmentVariables` con il prefisso.</span><span class="sxs-lookup"><span data-stu-id="db393-339">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="db393-340">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-340">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="db393-341">Chiamare il metodo di estensione `AddEnvironmentVariables` su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="db393-341">Call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="db393-342">Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="db393-342">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="db393-343">Se è necessario specificare la configurazione dell'app da variabili di ambiente aggiuntive, chiamare i provider aggiuntivi dell'app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> e chiamare `AddEnvironmentVariables` con il prefisso.</span><span class="sxs-lookup"><span data-stu-id="db393-343">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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
            // Call additional providers here as needed.
            // Call AddEnvironmentVariables last if you need to allow environment
            // variables to override values from other providers.
            .AddEnvironmentVariables(prefix: "PREFIX_")
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="db393-344">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-344">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="db393-345">Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="db393-345">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="db393-346">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="db393-346">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="db393-347">L'app di esempio 2.x consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include una chiamata a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="db393-347">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="db393-348">L'app di esempio 1.x chiama `AddEnvironmentVariables` per un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="db393-348">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="db393-349">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="db393-349">Run the sample app.</span></span> <span data-ttu-id="db393-350">Aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="db393-350">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="db393-351">Notare che l'output contiene la coppia chiave-valore per la variabile di ambiente `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="db393-351">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="db393-352">Il valore riflette l'ambiente in cui l'app è in esecuzione, in genere `Development` durante l'esecuzione locale.</span><span class="sxs-lookup"><span data-stu-id="db393-352">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="db393-353">Per mantenere breve l'elenco delle variabili di ambiente restituito dall'app, l'app filtra le variabili di ambiente includendo quelle che iniziano come segue:</span><span class="sxs-lookup"><span data-stu-id="db393-353">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="db393-354">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="db393-354">ASPNETCORE_</span></span>
* <span data-ttu-id="db393-355">urls</span><span class="sxs-lookup"><span data-stu-id="db393-355">urls</span></span>
* <span data-ttu-id="db393-356">Registrazione</span><span class="sxs-lookup"><span data-stu-id="db393-356">Logging</span></span>
* <span data-ttu-id="db393-357">ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="db393-357">ENVIRONMENT</span></span>
* <span data-ttu-id="db393-358">contentRoot</span><span class="sxs-lookup"><span data-stu-id="db393-358">contentRoot</span></span>
* <span data-ttu-id="db393-359">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="db393-359">AllowedHosts</span></span>
* <span data-ttu-id="db393-360">applicationName</span><span class="sxs-lookup"><span data-stu-id="db393-360">applicationName</span></span>
* <span data-ttu-id="db393-361">CommandLine</span><span class="sxs-lookup"><span data-stu-id="db393-361">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="db393-362">Se si desidera esporre tutte le variabili di ambiente disponibili all'app, modificare `FilteredConfiguration` in *Pages/Index.cshtml.cs* come segue:</span><span class="sxs-lookup"><span data-stu-id="db393-362">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="db393-363">Se si desidera esporre tutte le variabili di ambiente disponibili all'app, modificare `FilteredConfiguration` in *Controllers/HomeController.cs* come segue:</span><span class="sxs-lookup"><span data-stu-id="db393-363">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="db393-364">Prefissi</span><span class="sxs-lookup"><span data-stu-id="db393-364">Prefixes</span></span>

<span data-ttu-id="db393-365">Le variabili di ambiente caricate nella configurazione dell'app vengono filtrate quando si specifica un prefisso per il metodo `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="db393-365">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="db393-366">Ad esempio, per filtrare le variabili di ambiente in base al prefisso `CUSTOM_`, fornire il prefisso al provider di configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-366">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="db393-367">Il prefisso viene rimosso quando vengono create le coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="db393-367">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="db393-368">Il metodo di servizio statico `CreateDefaultBuilder` crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> per stabilire l'host dell'app.</span><span class="sxs-lookup"><span data-stu-id="db393-368">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="db393-369">Quando viene creato `WebHostBuilder`, la configurazione dell'host corrispondente viene individuata nelle variabili di ambiente con il prefisso `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="db393-369">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="db393-370">**Prefissi della stringa di connessione**</span><span class="sxs-lookup"><span data-stu-id="db393-370">**Connection string prefixes**</span></span>

<span data-ttu-id="db393-371">L'API di configurazione prevede regole di elaborazione speciali per quattro variabili di ambiente di stringa di connessione coinvolte nella configurazione di stringhe di connessione di Azure per l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="db393-371">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="db393-372">Le variabili di ambiente con i prefissi indicati nella tabella vengono caricate nell'app se non viene fornito alcun prefisso a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="db393-372">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="db393-373">Prefisso della stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="db393-373">Connection string prefix</span></span> | <span data-ttu-id="db393-374">Provider</span><span class="sxs-lookup"><span data-stu-id="db393-374">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="db393-375">Provider personalizzato</span><span class="sxs-lookup"><span data-stu-id="db393-375">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="db393-376">MySQL</span><span class="sxs-lookup"><span data-stu-id="db393-376">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="db393-377">Database SQL di Azure</span><span class="sxs-lookup"><span data-stu-id="db393-377">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="db393-378">SQL Server</span><span class="sxs-lookup"><span data-stu-id="db393-378">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="db393-379">Quando una variabile di ambiente viene individuata e caricata nella configurazione con uno qualsiasi dei quattro prefissi indicati nella tabella:</span><span class="sxs-lookup"><span data-stu-id="db393-379">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="db393-380">La chiave di configurazione viene creata rimuovendo il prefisso della variabile di ambiente e aggiungendo una sezione per la chiave di configurazione (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="db393-380">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="db393-381">Viene creata una nuova coppia chiave-valore della configurazione che rappresenta il provider di connessione al database (ad eccezione di `CUSTOMCONNSTR_`, che non ha un provider dichiarato).</span><span class="sxs-lookup"><span data-stu-id="db393-381">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="db393-382">Chiave della variabile di ambiente</span><span class="sxs-lookup"><span data-stu-id="db393-382">Environment variable key</span></span> | <span data-ttu-id="db393-383">Chiave di configurazione convertita</span><span class="sxs-lookup"><span data-stu-id="db393-383">Converted configuration key</span></span> | <span data-ttu-id="db393-384">Voce di configurazione del provider</span><span class="sxs-lookup"><span data-stu-id="db393-384">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="db393-385">Voce di configurazione non creata.</span><span class="sxs-lookup"><span data-stu-id="db393-385">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="db393-386">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="db393-386">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="db393-387">Valore: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="db393-387">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="db393-388">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="db393-388">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="db393-389">Valore: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="db393-389">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="db393-390">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="db393-390">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="db393-391">Valore: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="db393-391">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="db393-392">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="db393-392">File Configuration Provider</span></span>

<span data-ttu-id="db393-393"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> è la classe base per il caricamento della configurazione dal file system.</span><span class="sxs-lookup"><span data-stu-id="db393-393"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="db393-394">I provider di configurazione seguenti sono dedicati per specifici tipi di file:</span><span class="sxs-lookup"><span data-stu-id="db393-394">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="db393-395">Provider di configurazione INI</span><span class="sxs-lookup"><span data-stu-id="db393-395">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="db393-396">Provider di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="db393-396">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="db393-397">Provider di configurazione XML</span><span class="sxs-lookup"><span data-stu-id="db393-397">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="db393-398">Provider di configurazione INI</span><span class="sxs-lookup"><span data-stu-id="db393-398">INI Configuration Provider</span></span>

<span data-ttu-id="db393-399"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> carica la configurazione da coppie chiave-valore di file INI in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="db393-399">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="db393-400">Per attivare la configurazione di file INI, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="db393-400">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="db393-401">I due punti possono essere usati come delimitatore di sezione nella configurazione di file INI.</span><span class="sxs-lookup"><span data-stu-id="db393-401">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="db393-402">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="db393-402">Overloads permit specifying:</span></span>

* <span data-ttu-id="db393-403">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="db393-403">Whether the file is optional.</span></span>
* <span data-ttu-id="db393-404">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="db393-404">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="db393-405">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="db393-405">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="db393-406">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="db393-406">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="db393-407">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-407">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="db393-408">Quando si chiama `CreateDefaultBuilder`, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-408">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="db393-409">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-409">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="db393-410">Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="db393-410">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="db393-411">Un esempio generico di un file di configurazione INI:</span><span class="sxs-lookup"><span data-stu-id="db393-411">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="db393-412">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="db393-412">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="db393-413">section0:key0</span><span class="sxs-lookup"><span data-stu-id="db393-413">section0:key0</span></span>
* <span data-ttu-id="db393-414">section0:key1</span><span class="sxs-lookup"><span data-stu-id="db393-414">section0:key1</span></span>
* <span data-ttu-id="db393-415">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="db393-415">section1:subsection:key</span></span>
* <span data-ttu-id="db393-416">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="db393-416">section2:subsection0:key</span></span>
* <span data-ttu-id="db393-417">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="db393-417">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="db393-418">Provider di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="db393-418">JSON Configuration Provider</span></span>

<span data-ttu-id="db393-419">Il <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> carica la configurazione da coppie chiave-valore di file JSON in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="db393-419">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="db393-420">Per attivare la configurazione di file JSON, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="db393-420">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="db393-421">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="db393-421">Overloads permit specifying:</span></span>

* <span data-ttu-id="db393-422">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="db393-422">Whether the file is optional.</span></span>
* <span data-ttu-id="db393-423">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="db393-423">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="db393-424">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="db393-424">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="db393-425">`AddJsonFile` viene chiamato automaticamente due volte quando si inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="db393-425">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="db393-426">Il metodo viene chiamato per caricare la configurazione da:</span><span class="sxs-lookup"><span data-stu-id="db393-426">The method is called to load configuration from:</span></span>

* <span data-ttu-id="db393-427">*appsettings.json* &ndash; Questo file viene letto per primo.</span><span class="sxs-lookup"><span data-stu-id="db393-427">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="db393-428">La versione dell'ambiente del file può sostituire i valori forniti dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="db393-428">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="db393-429">*appsettings.{Environment}.json* &ndash; La versione dell'ambiente del file viene caricata in base a [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="db393-429">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="db393-430">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="db393-430">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="db393-431">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="db393-431">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="db393-432">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="db393-432">Environment variables.</span></span>
* <span data-ttu-id="db393-433">[Segreti utente (Secret Manager)](xref:security/app-secrets) (nell'ambiente Development).</span><span class="sxs-lookup"><span data-stu-id="db393-433">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="db393-434">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="db393-434">Command-line arguments.</span></span>

<span data-ttu-id="db393-435">Il provider di configurazione JSON viene stabilito per primo.</span><span class="sxs-lookup"><span data-stu-id="db393-435">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="db393-436">I segreti utente, le variabili di ambiente e gli argomenti della riga di comando sostituiscono quindi la configurazione impostata dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="db393-436">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="db393-437">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app per file diversi da *appsettings.json* e *appsettings.{Environment}.json*:</span><span class="sxs-lookup"><span data-stu-id="db393-437">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

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

<span data-ttu-id="db393-438">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-438">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="db393-439">È possibile chiamare direttamente il metodo di estensione `AddJsonFile` su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="db393-439">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="db393-440">Quando si chiama `CreateDefaultBuilder`, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-440">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="db393-441">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-441">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="db393-442">Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="db393-442">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="db393-443">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="db393-443">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="db393-444">L'app di esempio 2.x consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include due chiamate a `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="db393-444">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="db393-445">La configurazione viene caricata da *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="db393-445">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="db393-446">L'app di esempio 1.x chiama `AddJsonFile` due volte su un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="db393-446">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="db393-447">La configurazione viene caricata da *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="db393-447">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="db393-448">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="db393-448">Run the sample app.</span></span> <span data-ttu-id="db393-449">Aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="db393-449">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="db393-450">Notare che l'output contiene coppie chiave-valore per la configurazione mostrata nella tabella in base all'ambiente.</span><span class="sxs-lookup"><span data-stu-id="db393-450">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="db393-451">Le chiavi di configurazione di registrazione usano i due punti (`:`) come separatore gerarchico.</span><span class="sxs-lookup"><span data-stu-id="db393-451">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="db393-452">Chiave</span><span class="sxs-lookup"><span data-stu-id="db393-452">Key</span></span>                        | <span data-ttu-id="db393-453">Valore Development</span><span class="sxs-lookup"><span data-stu-id="db393-453">Development Value</span></span> | <span data-ttu-id="db393-454">Valore Production</span><span class="sxs-lookup"><span data-stu-id="db393-454">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="db393-455">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="db393-455">Logging:LogLevel:System</span></span>    | <span data-ttu-id="db393-456">Informazioni</span><span class="sxs-lookup"><span data-stu-id="db393-456">Information</span></span>       | <span data-ttu-id="db393-457">Informazioni</span><span class="sxs-lookup"><span data-stu-id="db393-457">Information</span></span>      |
| <span data-ttu-id="db393-458">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="db393-458">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="db393-459">Informazioni</span><span class="sxs-lookup"><span data-stu-id="db393-459">Information</span></span>       | <span data-ttu-id="db393-460">Informazioni</span><span class="sxs-lookup"><span data-stu-id="db393-460">Information</span></span>      |
| <span data-ttu-id="db393-461">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="db393-461">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="db393-462">Debug</span><span class="sxs-lookup"><span data-stu-id="db393-462">Debug</span></span>             | <span data-ttu-id="db393-463">Error</span><span class="sxs-lookup"><span data-stu-id="db393-463">Error</span></span>            |
| <span data-ttu-id="db393-464">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="db393-464">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="db393-465">Provider di configurazione XML</span><span class="sxs-lookup"><span data-stu-id="db393-465">XML Configuration Provider</span></span>

<span data-ttu-id="db393-466">Il <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> carica la configurazione da coppie chiave-valore di file XML in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="db393-466">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="db393-467">Per attivare la configurazione di file XML, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="db393-467">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="db393-468">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="db393-468">Overloads permit specifying:</span></span>

* <span data-ttu-id="db393-469">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="db393-469">Whether the file is optional.</span></span>
* <span data-ttu-id="db393-470">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="db393-470">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="db393-471">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="db393-471">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="db393-472">Il nodo radice del file di configurazione viene ignorato quando vengono create le coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="db393-472">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="db393-473">Non specificare una definizione DTD (Document Type Definition) o uno spazio dei nomi nel file.</span><span class="sxs-lookup"><span data-stu-id="db393-473">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="db393-474">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="db393-474">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="db393-475">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-475">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="db393-476">Quando si chiama `CreateDefaultBuilder`, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-476">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="db393-477">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-477">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="db393-478">Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="db393-478">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="db393-479">I file di configurazione XML possono usare nomi di elementi distinti per le sezioni ripetute:</span><span class="sxs-lookup"><span data-stu-id="db393-479">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="db393-480">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="db393-480">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="db393-481">section0:key0</span><span class="sxs-lookup"><span data-stu-id="db393-481">section0:key0</span></span>
* <span data-ttu-id="db393-482">section0:key1</span><span class="sxs-lookup"><span data-stu-id="db393-482">section0:key1</span></span>
* <span data-ttu-id="db393-483">section1:key0</span><span class="sxs-lookup"><span data-stu-id="db393-483">section1:key0</span></span>
* <span data-ttu-id="db393-484">section1:key1</span><span class="sxs-lookup"><span data-stu-id="db393-484">section1:key1</span></span>

<span data-ttu-id="db393-485">La ripetizione di elementi che usano lo stesso nome di elemento funziona se si usa l'attributo `name` per distinguere gli elementi:</span><span class="sxs-lookup"><span data-stu-id="db393-485">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="db393-486">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="db393-486">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="db393-487">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="db393-487">section:section0:key:key0</span></span>
* <span data-ttu-id="db393-488">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="db393-488">section:section0:key:key1</span></span>
* <span data-ttu-id="db393-489">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="db393-489">section:section1:key:key0</span></span>
* <span data-ttu-id="db393-490">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="db393-490">section:section1:key:key1</span></span>

<span data-ttu-id="db393-491">È possibile usare attributi per fornire valori:</span><span class="sxs-lookup"><span data-stu-id="db393-491">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="db393-492">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="db393-492">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="db393-493">key:attribute</span><span class="sxs-lookup"><span data-stu-id="db393-493">key:attribute</span></span>
* <span data-ttu-id="db393-494">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="db393-494">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="db393-495">Provider di configurazione KeyPerFile</span><span class="sxs-lookup"><span data-stu-id="db393-495">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="db393-496">Il <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> usa i file di una directory come coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="db393-496">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="db393-497">La chiave è il nome del file.</span><span class="sxs-lookup"><span data-stu-id="db393-497">The key is the file name.</span></span> <span data-ttu-id="db393-498">Il valore contiene il contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="db393-498">The value contains the file's contents.</span></span> <span data-ttu-id="db393-499">Il provider di configurazione KeyPerFile viene usato in scenari di hosting Docker.</span><span class="sxs-lookup"><span data-stu-id="db393-499">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="db393-500">Per attivare la configurazione chiave-per-file, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="db393-500">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="db393-501">Il `directoryPath` per i file deve essere un percorso assoluto.</span><span class="sxs-lookup"><span data-stu-id="db393-501">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="db393-502">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="db393-502">Overloads permit specifying:</span></span>

* <span data-ttu-id="db393-503">Un delegato `Action<KeyPerFileConfigurationSource>` che configura l'origine.</span><span class="sxs-lookup"><span data-stu-id="db393-503">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="db393-504">Se la directory è facoltativa e il percorso della directory.</span><span class="sxs-lookup"><span data-stu-id="db393-504">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="db393-505">Il doppio carattere di sottolineatura (`__`) viene usato come delimitatore per le chiavi di configurazione nei nomi dei file.</span><span class="sxs-lookup"><span data-stu-id="db393-505">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="db393-506">Ad esempio, il nome di file `Logging__LogLevel__System` produce la chiave di configurazione `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="db393-506">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="db393-507">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="db393-507">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="db393-508">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-508">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="db393-509">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="db393-509">Memory Configuration Provider</span></span>

<span data-ttu-id="db393-510">Il <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> usa una raccolta in memoria come coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="db393-510">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="db393-511">Per attivare la configurazione della raccolta in memoria, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="db393-511">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="db393-512">Il provider di configurazione può essere inizializzato con un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="db393-512">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="db393-513">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="db393-513">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="db393-514">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-514">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="db393-515">Quando si chiama `CreateDefaultBuilder`, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-515">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="db393-516">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-516">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="db393-517">Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="db393-517">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="db393-518">GetValue</span><span class="sxs-lookup"><span data-stu-id="db393-518">GetValue</span></span>

<span data-ttu-id="db393-519">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) estrae un valore dalla configurazione con una chiave specificata e lo converte nel tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="db393-519">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="db393-520">Un overload consente di specificare un valore predefinito se non viene trovata la chiave.</span><span class="sxs-lookup"><span data-stu-id="db393-520">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="db393-521">L'esempio seguente estrae il valore della stringa dalla configurazione con la chiave `NumberKey`, assegna al valore il tipo `int` e archivia il valore nella variabile `intValue`.</span><span class="sxs-lookup"><span data-stu-id="db393-521">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="db393-522">Se `NumberKey` non viene trovato nelle chiavi di configurazione `intValue` riceve il valore predefinito `99`:</span><span class="sxs-lookup"><span data-stu-id="db393-522">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="db393-523">GetSection, GetChildren ed Exists</span><span class="sxs-lookup"><span data-stu-id="db393-523">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="db393-524">Per gli esempi che seguono, prendere in considerazione il file JSON seguente.</span><span class="sxs-lookup"><span data-stu-id="db393-524">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="db393-525">Vengono trovate quattro chiavi in due sezioni, una delle quali include una coppia di sottosezioni:</span><span class="sxs-lookup"><span data-stu-id="db393-525">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="db393-526">Quando il file viene letto nella configurazione, vengono create le chiavi gerarchiche univoche seguenti per contenere i valori di configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-526">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="db393-527">section0:key0</span><span class="sxs-lookup"><span data-stu-id="db393-527">section0:key0</span></span>
* <span data-ttu-id="db393-528">section0:key1</span><span class="sxs-lookup"><span data-stu-id="db393-528">section0:key1</span></span>
* <span data-ttu-id="db393-529">section1:key0</span><span class="sxs-lookup"><span data-stu-id="db393-529">section1:key0</span></span>
* <span data-ttu-id="db393-530">section1:key1</span><span class="sxs-lookup"><span data-stu-id="db393-530">section1:key1</span></span>
* <span data-ttu-id="db393-531">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="db393-531">section2:subsection0:key0</span></span>
* <span data-ttu-id="db393-532">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="db393-532">section2:subsection0:key1</span></span>
* <span data-ttu-id="db393-533">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="db393-533">section2:subsection1:key0</span></span>
* <span data-ttu-id="db393-534">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="db393-534">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="db393-535">GetSection</span><span class="sxs-lookup"><span data-stu-id="db393-535">GetSection</span></span>

<span data-ttu-id="db393-536">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) estrae una sottosezione della configurazione con la chiave di sottosezione specificata.</span><span class="sxs-lookup"><span data-stu-id="db393-536">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="db393-537">Per restituire una <xref:Microsoft.Extensions.Configuration.IConfigurationSection> che contiene solo le coppie chiave-valore in `section1`, chiamare `GetSection` e fornire il nome della sezione:</span><span class="sxs-lookup"><span data-stu-id="db393-537">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="db393-538">In modo analogo, per ottenere i valori per le chiavi in `section2:subsection0`, chiamare `GetSection` e fornire il percorso della sezione:</span><span class="sxs-lookup"><span data-stu-id="db393-538">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="db393-539">`GetSection` non restituisce mai `null`.</span><span class="sxs-lookup"><span data-stu-id="db393-539">`GetSection` never returns `null`.</span></span> <span data-ttu-id="db393-540">Se non viene trovata una sezione corrispondente, viene restituita una `IConfigurationSection` vuota.</span><span class="sxs-lookup"><span data-stu-id="db393-540">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

### <a name="getchildren"></a><span data-ttu-id="db393-541">GetChildren</span><span class="sxs-lookup"><span data-stu-id="db393-541">GetChildren</span></span>

<span data-ttu-id="db393-542">Una chiamata a [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) per `section2` ottiene una `IEnumerable<IConfigurationSection>` che include:</span><span class="sxs-lookup"><span data-stu-id="db393-542">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="db393-543">Esistente</span><span class="sxs-lookup"><span data-stu-id="db393-543">Exists</span></span>

<span data-ttu-id="db393-544">Usare [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) per determinare se esiste una sezione di configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-544">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="db393-545">Considerati i dati di esempio, `sectionExists` è `false` perché non esiste una sezione `section2:subsection2` nei dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="db393-545">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="db393-546">Eseguire l'associazione a una classe</span><span class="sxs-lookup"><span data-stu-id="db393-546">Bind to a class</span></span>

<span data-ttu-id="db393-547">La configurazione può essere associata alle classi che rappresentano gruppi di impostazioni correlate usando il *modello di opzioni*.</span><span class="sxs-lookup"><span data-stu-id="db393-547">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="db393-548">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="db393-548">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="db393-549">I valori di configurazione vengono restituiti come stringhe, ma la chiamata di <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> consente la costruzione di oggetti [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="db393-549">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="db393-550">L'app di esempio contiene un modello `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="db393-550">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="db393-551">La sezione `starship` del file *starship.json* crea la configurazione quando l'app di esempio usa il provider di configurazione JSON per caricare la configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-551">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="db393-552">Vengono create le coppie chiave-valore della configurazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="db393-552">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="db393-553">Chiave</span><span class="sxs-lookup"><span data-stu-id="db393-553">Key</span></span>                   | <span data-ttu-id="db393-554">Value</span><span class="sxs-lookup"><span data-stu-id="db393-554">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="db393-555">starship:name</span><span class="sxs-lookup"><span data-stu-id="db393-555">starship:name</span></span>         | <span data-ttu-id="db393-556">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="db393-556">USS Enterprise</span></span>                                    |
| <span data-ttu-id="db393-557">starship:registry</span><span class="sxs-lookup"><span data-stu-id="db393-557">starship:registry</span></span>     | <span data-ttu-id="db393-558">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="db393-558">NCC-1701</span></span>                                          |
| <span data-ttu-id="db393-559">starship:class</span><span class="sxs-lookup"><span data-stu-id="db393-559">starship:class</span></span>        | <span data-ttu-id="db393-560">Constitution</span><span class="sxs-lookup"><span data-stu-id="db393-560">Constitution</span></span>                                      |
| <span data-ttu-id="db393-561">starship:length</span><span class="sxs-lookup"><span data-stu-id="db393-561">starship:length</span></span>       | <span data-ttu-id="db393-562">304.8</span><span class="sxs-lookup"><span data-stu-id="db393-562">304.8</span></span>                                             |
| <span data-ttu-id="db393-563">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="db393-563">starship:commissioned</span></span> | <span data-ttu-id="db393-564">False</span><span class="sxs-lookup"><span data-stu-id="db393-564">False</span></span>                                             |
| <span data-ttu-id="db393-565">trademark</span><span class="sxs-lookup"><span data-stu-id="db393-565">trademark</span></span>             | <span data-ttu-id="db393-566">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="db393-566">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="db393-567">L'app di esempio chiama `GetSection` con la chiave `starship`.</span><span class="sxs-lookup"><span data-stu-id="db393-567">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="db393-568">Le coppie chiave-valore `starship` sono isolate.</span><span class="sxs-lookup"><span data-stu-id="db393-568">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="db393-569">Il metodo `Bind` viene chiamato sulla sottosezione passando un'istanza della classe `Starship`.</span><span class="sxs-lookup"><span data-stu-id="db393-569">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="db393-570">Dopo aver associato i valori dell'istanza, l'istanza viene assegnata a una proprietà per il rendering:</span><span class="sxs-lookup"><span data-stu-id="db393-570">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="db393-571">Associazione a un oggetto grafico</span><span class="sxs-lookup"><span data-stu-id="db393-571">Bind to an object graph</span></span>

<span data-ttu-id="db393-572"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> è in grado di associare un intero oggetto grafico POCO.</span><span class="sxs-lookup"><span data-stu-id="db393-572"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="db393-573">L'esempio contiene un modello `TvShow` il cui oggetto grafico include `Metadata` e le classi `Actors` (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="db393-573">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="db393-574">L'app di esempio ha un file *tvshow.xml* che contiene i dati di configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-574">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="db393-575">La configurazione viene associata all'intero oggetto grafico `TvShow` con il metodo `Bind`.</span><span class="sxs-lookup"><span data-stu-id="db393-575">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="db393-576">L'istanza associata viene assegnata a una proprietà per il rendering:</span><span class="sxs-lookup"><span data-stu-id="db393-576">The bound instance is assigned to a property for rendering:</span></span>

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

<span data-ttu-id="db393-577">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) associa e restituisce il tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="db393-577">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="db393-578">`Get<T>` può essere più comodo che usare `Bind`.</span><span class="sxs-lookup"><span data-stu-id="db393-578">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="db393-579">Il codice seguente mostra come usare `Get<T>` con l'esempio precedente, che consente di assegnare l'istanza associata direttamente alla proprietà usata per il rendering:</span><span class="sxs-lookup"><span data-stu-id="db393-579">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="db393-580">Associare una matrice a una classe</span><span class="sxs-lookup"><span data-stu-id="db393-580">Bind an array to a class</span></span>

<span data-ttu-id="db393-581">*L'app di esempio dimostra i concetti spiegati in questa sezione.*</span><span class="sxs-lookup"><span data-stu-id="db393-581">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="db393-582">Il <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supporta l'associazione di matrici agli oggetti usando gli indici delle matrici nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="db393-582">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="db393-583">Qualsiasi formato di matrice che espone un segmento chiave numerico (`:0:`, `:1:`, &hellip; `:{n}:`) supporta l'associazione di matrici a una matrice di classi POCO.</span><span class="sxs-lookup"><span data-stu-id="db393-583">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="db393-584">L'associazione viene fornita per convenzione.</span><span class="sxs-lookup"><span data-stu-id="db393-584">Binding is provided by convention.</span></span> <span data-ttu-id="db393-585">I provider di configurazione personalizzati non devono implementare l'associazione di matrici.</span><span class="sxs-lookup"><span data-stu-id="db393-585">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="db393-586">**Elaborazione di matrici in memoria**</span><span class="sxs-lookup"><span data-stu-id="db393-586">**In-memory array processing**</span></span>

<span data-ttu-id="db393-587">Prendere in considerazione le chiavi di configurazione e i valori indicati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="db393-587">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="db393-588">Chiave</span><span class="sxs-lookup"><span data-stu-id="db393-588">Key</span></span>             | <span data-ttu-id="db393-589">Value</span><span class="sxs-lookup"><span data-stu-id="db393-589">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="db393-590">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="db393-590">array:entries:0</span></span> | <span data-ttu-id="db393-591">value0</span><span class="sxs-lookup"><span data-stu-id="db393-591">value0</span></span> |
| <span data-ttu-id="db393-592">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="db393-592">array:entries:1</span></span> | <span data-ttu-id="db393-593">value1</span><span class="sxs-lookup"><span data-stu-id="db393-593">value1</span></span> |
| <span data-ttu-id="db393-594">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="db393-594">array:entries:2</span></span> | <span data-ttu-id="db393-595">value2</span><span class="sxs-lookup"><span data-stu-id="db393-595">value2</span></span> |
| <span data-ttu-id="db393-596">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="db393-596">array:entries:4</span></span> | <span data-ttu-id="db393-597">value4</span><span class="sxs-lookup"><span data-stu-id="db393-597">value4</span></span> |
| <span data-ttu-id="db393-598">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="db393-598">array:entries:5</span></span> | <span data-ttu-id="db393-599">value5</span><span class="sxs-lookup"><span data-stu-id="db393-599">value5</span></span> |

<span data-ttu-id="db393-600">Queste chiavi e i valori vengono caricati nell'app di esempio usando il provider di configurazione della memoria:</span><span class="sxs-lookup"><span data-stu-id="db393-600">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="db393-601">La matrice ignora un valore per l'indice &num;3.</span><span class="sxs-lookup"><span data-stu-id="db393-601">The array skips a value for index &num;3.</span></span> <span data-ttu-id="db393-602">Lo strumento di associazione di configurazione non è in grado di associare valori null o di creare voci null negli oggetti associati, situazione che diventerà chiara tra poco quando viene illustrato il risultato dell'associazione di questa matrice a un oggetto.</span><span class="sxs-lookup"><span data-stu-id="db393-602">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="db393-603">Nell'app di esempio, è disponibile una classe POCO per i dati di configurazione associati:</span><span class="sxs-lookup"><span data-stu-id="db393-603">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="db393-604">I dati di configurazione sono associati all'oggetto:</span><span class="sxs-lookup"><span data-stu-id="db393-604">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="db393-605">È possibile usare anche la sintassi [ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*), che produce codice più compatto:</span><span class="sxs-lookup"><span data-stu-id="db393-605">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="db393-606">L'oggetto associato, un'istanza di `ArrayExample`, riceve i dati di matrice dalla configurazione.</span><span class="sxs-lookup"><span data-stu-id="db393-606">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="db393-607">Indice di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="db393-607">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="db393-608">Valore di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="db393-608">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="db393-609">0</span><span class="sxs-lookup"><span data-stu-id="db393-609">0</span></span>                            | <span data-ttu-id="db393-610">value0</span><span class="sxs-lookup"><span data-stu-id="db393-610">value0</span></span>                       |
| <span data-ttu-id="db393-611">1</span><span class="sxs-lookup"><span data-stu-id="db393-611">1</span></span>                            | <span data-ttu-id="db393-612">value1</span><span class="sxs-lookup"><span data-stu-id="db393-612">value1</span></span>                       |
| <span data-ttu-id="db393-613">2</span><span class="sxs-lookup"><span data-stu-id="db393-613">2</span></span>                            | <span data-ttu-id="db393-614">value2</span><span class="sxs-lookup"><span data-stu-id="db393-614">value2</span></span>                       |
| <span data-ttu-id="db393-615">3</span><span class="sxs-lookup"><span data-stu-id="db393-615">3</span></span>                            | <span data-ttu-id="db393-616">value4</span><span class="sxs-lookup"><span data-stu-id="db393-616">value4</span></span>                       |
| <span data-ttu-id="db393-617">4</span><span class="sxs-lookup"><span data-stu-id="db393-617">4</span></span>                            | <span data-ttu-id="db393-618">value5</span><span class="sxs-lookup"><span data-stu-id="db393-618">value5</span></span>                       |

<span data-ttu-id="db393-619">L'indice &num;3 nell'oggetto associato contiene i dati di configurazione per la chiave di configurazione `array:4` e il relativo valore `value4`.</span><span class="sxs-lookup"><span data-stu-id="db393-619">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="db393-620">Quando i dati di configurazione contenenti una matrice vengono associati, gli indici di matrice nelle chiavi di configurazione vengono usati semplicemente per scorrere i dati di configurazione quando si crea l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="db393-620">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="db393-621">Un valore null non può essere mantenuto nei dati di configurazione e una voce con valore null non viene creata in un oggetto associato quando una matrice nelle chiavi di configurazione ignora uno o più indici.</span><span class="sxs-lookup"><span data-stu-id="db393-621">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="db393-622">L'elemento di configurazione mancante per l'indice &num;3 può essere fornito prima dell'associazione all'istanza `ArrayExample` da qualsiasi provider di configurazione che produce la coppia chiave-valore corretta nella configurazione.</span><span class="sxs-lookup"><span data-stu-id="db393-622">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="db393-623">Se il codice di esempio include un provider di configurazione JSON aggiuntivo con la coppia chiave-valore mancante, `ArrayExample.Entries` corrisponde alla matrice di configurazione completa:</span><span class="sxs-lookup"><span data-stu-id="db393-623">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="db393-624">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="db393-624">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="db393-625">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="db393-625">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="db393-626">Nel costruttore `Startup`:</span><span class="sxs-lookup"><span data-stu-id="db393-626">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="db393-627">La coppia chiave-valore mostrata nella tabella viene caricata nella configurazione.</span><span class="sxs-lookup"><span data-stu-id="db393-627">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="db393-628">Chiave</span><span class="sxs-lookup"><span data-stu-id="db393-628">Key</span></span>             | <span data-ttu-id="db393-629">Value</span><span class="sxs-lookup"><span data-stu-id="db393-629">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="db393-630">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="db393-630">array:entries:3</span></span> | <span data-ttu-id="db393-631">value3</span><span class="sxs-lookup"><span data-stu-id="db393-631">value3</span></span> |

<span data-ttu-id="db393-632">Se l'istanza della classe `ArrayExample` viene associata dopo che il provider di configurazione JSON include la voce per l'indice &num;3, la matrice `ArrayExample.Entries` include il valore.</span><span class="sxs-lookup"><span data-stu-id="db393-632">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="db393-633">Indice di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="db393-633">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="db393-634">Valore di `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="db393-634">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="db393-635">0</span><span class="sxs-lookup"><span data-stu-id="db393-635">0</span></span>                            | <span data-ttu-id="db393-636">value0</span><span class="sxs-lookup"><span data-stu-id="db393-636">value0</span></span>                       |
| <span data-ttu-id="db393-637">1</span><span class="sxs-lookup"><span data-stu-id="db393-637">1</span></span>                            | <span data-ttu-id="db393-638">value1</span><span class="sxs-lookup"><span data-stu-id="db393-638">value1</span></span>                       |
| <span data-ttu-id="db393-639">2</span><span class="sxs-lookup"><span data-stu-id="db393-639">2</span></span>                            | <span data-ttu-id="db393-640">value2</span><span class="sxs-lookup"><span data-stu-id="db393-640">value2</span></span>                       |
| <span data-ttu-id="db393-641">3</span><span class="sxs-lookup"><span data-stu-id="db393-641">3</span></span>                            | <span data-ttu-id="db393-642">value3</span><span class="sxs-lookup"><span data-stu-id="db393-642">value3</span></span>                       |
| <span data-ttu-id="db393-643">4</span><span class="sxs-lookup"><span data-stu-id="db393-643">4</span></span>                            | <span data-ttu-id="db393-644">value4</span><span class="sxs-lookup"><span data-stu-id="db393-644">value4</span></span>                       |
| <span data-ttu-id="db393-645">5</span><span class="sxs-lookup"><span data-stu-id="db393-645">5</span></span>                            | <span data-ttu-id="db393-646">value5</span><span class="sxs-lookup"><span data-stu-id="db393-646">value5</span></span>                       |

<span data-ttu-id="db393-647">**Elaborazione di matrice JSON**</span><span class="sxs-lookup"><span data-stu-id="db393-647">**JSON array processing**</span></span>

<span data-ttu-id="db393-648">Se un file JSON contiene una matrice, vengono create chiavi di configurazione per gli elementi della matrice con un indice di sezione in base zero.</span><span class="sxs-lookup"><span data-stu-id="db393-648">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="db393-649">Nel file di configurazione seguente, `subsection` è una matrice:</span><span class="sxs-lookup"><span data-stu-id="db393-649">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="db393-650">Il provider di configurazione JSON legge i dati di configurazione nelle coppie chiave-valore seguenti:</span><span class="sxs-lookup"><span data-stu-id="db393-650">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="db393-651">Chiave</span><span class="sxs-lookup"><span data-stu-id="db393-651">Key</span></span>                     | <span data-ttu-id="db393-652">Value</span><span class="sxs-lookup"><span data-stu-id="db393-652">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="db393-653">json_array:key</span><span class="sxs-lookup"><span data-stu-id="db393-653">json_array:key</span></span>          | <span data-ttu-id="db393-654">valueA</span><span class="sxs-lookup"><span data-stu-id="db393-654">valueA</span></span> |
| <span data-ttu-id="db393-655">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="db393-655">json_array:subsection:0</span></span> | <span data-ttu-id="db393-656">valueB</span><span class="sxs-lookup"><span data-stu-id="db393-656">valueB</span></span> |
| <span data-ttu-id="db393-657">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="db393-657">json_array:subsection:1</span></span> | <span data-ttu-id="db393-658">valueC</span><span class="sxs-lookup"><span data-stu-id="db393-658">valueC</span></span> |
| <span data-ttu-id="db393-659">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="db393-659">json_array:subsection:2</span></span> | <span data-ttu-id="db393-660">valueD</span><span class="sxs-lookup"><span data-stu-id="db393-660">valueD</span></span> |

<span data-ttu-id="db393-661">Nell'app di esempio, la classe POCO seguente è disponibile per associare le coppie chiave-valore della configurazione:</span><span class="sxs-lookup"><span data-stu-id="db393-661">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="db393-662">Dopo l'associazione, `JsonArrayExample.Key` contiene il valore `valueA`.</span><span class="sxs-lookup"><span data-stu-id="db393-662">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="db393-663">I valori di sottosezione vengono archiviati nella proprietà matrice POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="db393-663">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="db393-664">Indice di `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="db393-664">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="db393-665">Valore di `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="db393-665">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="db393-666">0</span><span class="sxs-lookup"><span data-stu-id="db393-666">0</span></span>                                   | <span data-ttu-id="db393-667">valueB</span><span class="sxs-lookup"><span data-stu-id="db393-667">valueB</span></span>                              |
| <span data-ttu-id="db393-668">1</span><span class="sxs-lookup"><span data-stu-id="db393-668">1</span></span>                                   | <span data-ttu-id="db393-669">valueC</span><span class="sxs-lookup"><span data-stu-id="db393-669">valueC</span></span>                              |
| <span data-ttu-id="db393-670">2</span><span class="sxs-lookup"><span data-stu-id="db393-670">2</span></span>                                   | <span data-ttu-id="db393-671">valueD</span><span class="sxs-lookup"><span data-stu-id="db393-671">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="db393-672">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="db393-672">Custom configuration provider</span></span>

<span data-ttu-id="db393-673">L'app di esempio dimostra come creare un provider di configurazione di base che legge le coppie chiave-valore di configurazione da un database usando [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="db393-673">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="db393-674">Il provider ha le caratteristiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="db393-674">The provider has the following characteristics:</span></span>

* <span data-ttu-id="db393-675">Il database in memoria di Entity Framework viene usato a scopo dimostrativo.</span><span class="sxs-lookup"><span data-stu-id="db393-675">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="db393-676">Per usare un database che richiede una stringa di connessione, implementare un `ConfigurationBuilder` secondario per fornire la stringa di connessione da un altro provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="db393-676">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="db393-677">Il provider legge una tabella di database in una configurazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="db393-677">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="db393-678">Il provider non esegue una query sul database per ogni chiave.</span><span class="sxs-lookup"><span data-stu-id="db393-678">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="db393-679">Il ricaricamento in caso di modifica non è implementato, quindi l'aggiornamento del database dopo l'avvio dell'app non ha alcun effetto sulla configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="db393-679">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="db393-680">Definire un'entità `EFConfigurationValue` per l'archiviazione dei valori di configurazione nel database.</span><span class="sxs-lookup"><span data-stu-id="db393-680">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="db393-681">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="db393-681">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="db393-682">Aggiungere `EFConfigurationContext` per archiviare i valori configurati e accedervi.</span><span class="sxs-lookup"><span data-stu-id="db393-682">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="db393-683">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="db393-683">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="db393-684">Creare una classe che implementi <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="db393-684">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="db393-685">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="db393-685">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="db393-686">Creare il provider di configurazione personalizzato ereditando da <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="db393-686">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="db393-687">Il provider di configurazione inizializza il database quando è vuoto.</span><span class="sxs-lookup"><span data-stu-id="db393-687">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="db393-688">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="db393-688">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="db393-689">Un metodo di estensione `AddEFConfiguration` consente di aggiungere l'origine di configurazione a un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="db393-689">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="db393-690">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="db393-690">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="db393-691">L'esempio di codice seguente mostra come usare il `EFConfigurationProvider` personalizzato in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="db393-691">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="db393-692">Accedere alla configurazione durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="db393-692">Access configuration during startup</span></span>

<span data-ttu-id="db393-693">Inserire `IConfiguration` nel costruttore `Startup` per accedere ai valori di configurazione in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="db393-693">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="db393-694">Per accedere alla configurazione in `Startup.Configure`, inserire `IConfiguration` direttamente nel metodo o usare l'istanza dal costruttore:</span><span class="sxs-lookup"><span data-stu-id="db393-694">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="db393-695">Per un esempio di accesso alla configurazione usando metodi di servizio di avvio, vedere [Avvio dell'applicazione: Metodi pratici](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="db393-695">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="db393-696">Accedere alla configurazione in una pagina Razor Pages o in una visualizzazione MVC</span><span class="sxs-lookup"><span data-stu-id="db393-696">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="db393-697">Per accedere alle impostazioni di configurazione in una pagina Razor Pages o in una visualizzazione MVC, aggiungere un [direttiva using](xref:mvc/views/razor#using) ([Informazioni di riferimento su C#: direttiva using](/dotnet/csharp/language-reference/keywords/using-directive)) per lo [spazio dei nomi Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) e inserire <xref:Microsoft.Extensions.Configuration.IConfiguration> nella pagina o nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="db393-697">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="db393-698">In una pagina Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="db393-698">In a Razor Pages page:</span></span>

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

<span data-ttu-id="db393-699">In una visualizzazione MVC:</span><span class="sxs-lookup"><span data-stu-id="db393-699">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="db393-700">Aggiungere la configurazione da un assembly esterno</span><span class="sxs-lookup"><span data-stu-id="db393-700">Add configuration from an external assembly</span></span>

<span data-ttu-id="db393-701">Un'implementazione <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> consente l'aggiunta di miglioramenti a un'app all'avvio, da un assembly esterno alla classe `Startup` dell'app.</span><span class="sxs-lookup"><span data-stu-id="db393-701">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="db393-702">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="db393-702">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="db393-703">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="db393-703">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* <span data-ttu-id="db393-704">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/) (Configurazione Microsoft in dettaglio)</span><span class="sxs-lookup"><span data-stu-id="db393-704">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)</span></span>
