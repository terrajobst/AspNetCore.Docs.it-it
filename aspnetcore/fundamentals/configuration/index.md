---
title: Configurazione in ASP.NET Core
author: guardrex
description: Informazioni su come usare l'API di configurazione per configurare un'app ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/09/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 35f283becd156da22a4d9d2034055ee79b75ffda
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49326173"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="4d4ea-103">Configurazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4d4ea-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="4d4ea-104">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4d4ea-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4d4ea-105">La configurazione delle app in ASP.NET Core si basa su coppie chiave-valore stabilite dai *provider di configurazione*.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="4d4ea-106">I provider di configurazione leggono i dati di configurazione in coppie chiave-valore da un'ampia gamma di origini di configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="4d4ea-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="4d4ea-107">Azure Key Vault</span></span>
* <span data-ttu-id="4d4ea-108">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="4d4ea-108">Command-line arguments</span></span>
* <span data-ttu-id="4d4ea-109">Provider personalizzati (installati o creati)</span><span class="sxs-lookup"><span data-stu-id="4d4ea-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="4d4ea-110">File della directory</span><span class="sxs-lookup"><span data-stu-id="4d4ea-110">Directory files</span></span>
* <span data-ttu-id="4d4ea-111">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="4d4ea-111">Environment variables</span></span>
* <span data-ttu-id="4d4ea-112">Oggetti .NET in memoria</span><span class="sxs-lookup"><span data-stu-id="4d4ea-112">In-memory .NET objects</span></span>
* <span data-ttu-id="4d4ea-113">File di impostazioni</span><span class="sxs-lookup"><span data-stu-id="4d4ea-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="4d4ea-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="4d4ea-114">Azure Key Vault</span></span>
* <span data-ttu-id="4d4ea-115">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="4d4ea-115">Command-line arguments</span></span>
* <span data-ttu-id="4d4ea-116">Provider personalizzati (installati o creati)</span><span class="sxs-lookup"><span data-stu-id="4d4ea-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="4d4ea-117">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="4d4ea-117">Environment variables</span></span>
* <span data-ttu-id="4d4ea-118">Oggetti .NET in memoria</span><span class="sxs-lookup"><span data-stu-id="4d4ea-118">In-memory .NET objects</span></span>
* <span data-ttu-id="4d4ea-119">File di impostazioni</span><span class="sxs-lookup"><span data-stu-id="4d4ea-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="4d4ea-120">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="4d4ea-120">Command-line arguments</span></span>
* <span data-ttu-id="4d4ea-121">Provider personalizzati (installati o creati)</span><span class="sxs-lookup"><span data-stu-id="4d4ea-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="4d4ea-122">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="4d4ea-122">Environment variables</span></span>
* <span data-ttu-id="4d4ea-123">Oggetti .NET in memoria</span><span class="sxs-lookup"><span data-stu-id="4d4ea-123">In-memory .NET objects</span></span>
* <span data-ttu-id="4d4ea-124">File di impostazioni</span><span class="sxs-lookup"><span data-stu-id="4d4ea-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="4d4ea-125">Il *modello di opzioni* è un'estensione dei concetti di configurazione descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="4d4ea-126">Le opzioni usano le classi per rappresentare i gruppi di impostazioni correlate.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="4d4ea-127">Per altre informazioni sull'uso del modello di opzioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="4d4ea-128">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4d4ea-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4d4ea-129">Gli esempi in questo argomento si basano su:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-129">The examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="4d4ea-130">Impostazione del percorso base dell'app con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-130">Setting the base path of the app with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="4d4ea-131">`SetBasePath` viene resa disponibile per un'app facendo riferimento al pacchetto [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-131">`SetBasePath` is made available to an app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="4d4ea-132">Risoluzione di sezioni dei file di configurazione con <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-132">Resolving sections of configuration files with <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span></span> <span data-ttu-id="4d4ea-133">`GetSection` viene resa disponibile per un'app facendo riferimento al pacchetto [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-133">`GetSection` is made available to an app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="4d4ea-134">Associazione della configurazione a classi .NET con <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> e [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-134">Binding configuration to .NET classes with <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> and [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span></span> <span data-ttu-id="4d4ea-135">`Bind` e `Get<T>` vengono rese disponibili per un'app facendo riferimento al pacchetto [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-135">`Bind` and `Get<T>` are made available to an app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span> <span data-ttu-id="4d4ea-136">`Get<T>` è disponibile in ASP.NET Core 1.1 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-136">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4d4ea-137">Questi tre pacchetti sono inclusi nel [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-137">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="4d4ea-138">Questi tre pacchetti sono inclusi nel [metapacchetto Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-138">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="4d4ea-139">Host e configurazione delle app</span><span class="sxs-lookup"><span data-stu-id="4d4ea-139">Host vs. app configuration</span></span>

<span data-ttu-id="4d4ea-140">Prima che l'app venga configurata e avviata, viene configurato e avviato un *host*.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-140">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="4d4ea-141">L'host è responsabile della gestione dell'avvio e della durata delle app.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-141">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="4d4ea-142">Sia l'app che l'host vengono configurati tramite i provider di configurazione descritti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-142">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="4d4ea-143">Le coppie chiave-valore di configurazione dell'host diventano parte della configurazione globale dell'app.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-143">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="4d4ea-144">Per altre informazioni su come vengono usati i provider di configurazione quando viene creato l'host e sugli effetti delle origini di configurazione sull'host di configurazione, vedere <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-144">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="security"></a><span data-ttu-id="4d4ea-145">Sicurezza</span><span class="sxs-lookup"><span data-stu-id="4d4ea-145">Security</span></span>

<span data-ttu-id="4d4ea-146">Adottare le procedure consigliate seguenti:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-146">Adopt the following best practices:</span></span>

* <span data-ttu-id="4d4ea-147">Non archiviare mai la password o altri dati sensibili nel codice del provider di configurazione o in file di configurazione di testo normale.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-147">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="4d4ea-148">Non usare i segreti di produzione in ambienti di sviluppo o di test.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-148">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="4d4ea-149">Specificare i segreti all'esterno del progetto in modo che non possano essere inavvertitamente inviati a un repository del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-149">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="4d4ea-150">Altre informazioni su [come usare più ambienti](xref:fundamentals/environments) e la gestione dell'[archiviazione sicura dei segreti delle app durante lo sviluppo con Secret Manager](xref:security/app-secrets) (include consigli sull'uso delle variabili di ambiente per l'archiviazione di dati sensibili).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-150">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="4d4ea-151">Secret Manager usa il provider di configurazione dei file per archiviare i segreti utente in un file JSON nel sistema locale.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-151">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="4d4ea-152">Il provider di configurazione dei file è descritto più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-152">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="4d4ea-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) è una delle opzioni per l'archiviazione sicura dei segreti delle app.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="4d4ea-154">Per ulteriori informazioni, vedere <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-154">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="4d4ea-155">Dati di configurazione gerarchici</span><span class="sxs-lookup"><span data-stu-id="4d4ea-155">Hierarchical configuration data</span></span>

<span data-ttu-id="4d4ea-156">L'API di configurazione è in grado di mantenere i dati di configurazione gerarchici rendendoli flat grazie all'uso di un delimitatore nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-156">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="4d4ea-157">Nel file JSON seguente, esistono quattro chiavi in una gerarchia strutturata di due sezioni:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-157">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="4d4ea-158">Quando il file viene letto nella configurazione, vengono create chiavi univoche per mantenere la struttura di dati gerarchici originale dell'origine di configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-158">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="4d4ea-159">Le sezioni e le chiavi vengono rese flat usando due punti (`:`) per mantenere la struttura originale:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-159">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="4d4ea-160">section0:key0</span><span class="sxs-lookup"><span data-stu-id="4d4ea-160">section0:key0</span></span>
* <span data-ttu-id="4d4ea-161">section0:key1</span><span class="sxs-lookup"><span data-stu-id="4d4ea-161">section0:key1</span></span>
* <span data-ttu-id="4d4ea-162">section1:key0</span><span class="sxs-lookup"><span data-stu-id="4d4ea-162">section1:key0</span></span>
* <span data-ttu-id="4d4ea-163">section1:key1</span><span class="sxs-lookup"><span data-stu-id="4d4ea-163">section1:key1</span></span>

<span data-ttu-id="4d4ea-164">I metodi <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> e <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> sono disponibili per isolare le sezioni e gli elementi figlio di una sezione nei dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="4d4ea-165">Questi metodi sono descritti più avanti in [GetSection, GetChildren ed Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-165">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="4d4ea-166">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="4d4ea-166">Conventions</span></span>

<span data-ttu-id="4d4ea-167">All'avvio dell'app, le origini di configurazione vengono lette nell'ordine con cui sono specificati i rispettivi provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-167">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="4d4ea-168">I provider di configurazione dei file supportano il ricaricamento della configurazione quando un file di impostazioni sottostante viene modificato dopo l'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-168">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="4d4ea-169">Il provider di configurazione dei file è descritto più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-169">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="4d4ea-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> è disponibile nel contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) dell'app.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="4d4ea-171">I provider di configurazione non possono usare l'inserimento delle dipendenze, perché non è disponibile quando vengono configurati dall'host.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="4d4ea-172">Le chiavi di configurazione adottano le convenzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="4d4ea-173">Per le chiavi non viene fatta distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-173">Keys are case-insensitive.</span></span> <span data-ttu-id="4d4ea-174">Ad esempio, `ConnectionString` e `connectionstring` vengono considerate chiavi equivalenti.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="4d4ea-175">Se un valore per la stessa chiave viene impostato dallo stesso provider di configurazione o da provider diversi, viene usato l'ultimo valore impostato per la chiave.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="4d4ea-176">Chiavi gerarchiche</span><span class="sxs-lookup"><span data-stu-id="4d4ea-176">Hierarchical keys</span></span>
  * <span data-ttu-id="4d4ea-177">Nell'ambito dell'API di configurazione, il separatore due punti (`:`) funziona in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="4d4ea-178">Nelle variabili di ambiente, un separatore due punti potrebbe non funzionare in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="4d4ea-179">Il doppio carattere di sottolineatura (`__`) è supportato da tutte le piattaforme e viene convertito in due punti.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="4d4ea-180">In Azure Key Vault, le chiavi gerarchiche usano `--` (due trattini) come separatore.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="4d4ea-181">È necessario fornire il codice per sostituire i trattini con due punti quando i segreti vengono caricati nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="4d4ea-182">Il <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supporta l'associazione di matrici agli oggetti usando gli indici delle matrici nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="4d4ea-183">L'associazione di matrici è descritta nella sezione [Associare una matrice a una classe](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="4d4ea-184">I valori di configurazione adottano le convenzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="4d4ea-185">I valori sono stringhe.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-185">Values are strings.</span></span>
* <span data-ttu-id="4d4ea-186">I valori null non possono essere archiviati nella configurazione o associati a oggetti.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="4d4ea-187">Provider</span><span class="sxs-lookup"><span data-stu-id="4d4ea-187">Providers</span></span>

<span data-ttu-id="4d4ea-188">La tabella seguente mostra i provider di configurazione disponibili per le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="4d4ea-189">Provider</span><span class="sxs-lookup"><span data-stu-id="4d4ea-189">Provider</span></span> | <span data-ttu-id="4d4ea-190">Fornisce la configurazione da&hellip;</span><span class="sxs-lookup"><span data-stu-id="4d4ea-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="4d4ea-191">[Provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="4d4ea-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="4d4ea-192">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="4d4ea-192">Azure Key Vault</span></span> |
| [<span data-ttu-id="4d4ea-193">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="4d4ea-193">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="4d4ea-194">Parametri della riga di comando</span><span class="sxs-lookup"><span data-stu-id="4d4ea-194">Command-line parameters</span></span> |
| [<span data-ttu-id="4d4ea-195">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="4d4ea-195">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="4d4ea-196">Origine personalizzata</span><span class="sxs-lookup"><span data-stu-id="4d4ea-196">Custom source</span></span> |
| [<span data-ttu-id="4d4ea-197">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="4d4ea-197">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="4d4ea-198">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="4d4ea-198">Environment variables</span></span> |
| [<span data-ttu-id="4d4ea-199">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="4d4ea-199">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="4d4ea-200">File (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="4d4ea-200">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="4d4ea-201">Provider di configurazione chiave-per-file</span><span class="sxs-lookup"><span data-stu-id="4d4ea-201">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="4d4ea-202">File della directory</span><span class="sxs-lookup"><span data-stu-id="4d4ea-202">Directory files</span></span> |
| [<span data-ttu-id="4d4ea-203">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="4d4ea-203">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="4d4ea-204">Raccolte in memoria</span><span class="sxs-lookup"><span data-stu-id="4d4ea-204">In-memory collections</span></span> |
| <span data-ttu-id="4d4ea-205">[Segreti utente (Secret Manager)](xref:security/app-secrets) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="4d4ea-205">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="4d4ea-206">File nella directory dei profili utente</span><span class="sxs-lookup"><span data-stu-id="4d4ea-206">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="4d4ea-207">Provider</span><span class="sxs-lookup"><span data-stu-id="4d4ea-207">Provider</span></span> | <span data-ttu-id="4d4ea-208">Fornisce la configurazione da&hellip;</span><span class="sxs-lookup"><span data-stu-id="4d4ea-208">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="4d4ea-209">[Provider di configurazione di Azure Key Vault](xref:security/key-vault-configuration) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="4d4ea-209">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="4d4ea-210">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="4d4ea-210">Azure Key Vault</span></span> |
| [<span data-ttu-id="4d4ea-211">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="4d4ea-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="4d4ea-212">Parametri della riga di comando</span><span class="sxs-lookup"><span data-stu-id="4d4ea-212">Command-line parameters</span></span> |
| [<span data-ttu-id="4d4ea-213">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="4d4ea-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="4d4ea-214">Origine personalizzata</span><span class="sxs-lookup"><span data-stu-id="4d4ea-214">Custom source</span></span> |
| [<span data-ttu-id="4d4ea-215">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="4d4ea-215">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="4d4ea-216">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="4d4ea-216">Environment variables</span></span> |
| [<span data-ttu-id="4d4ea-217">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="4d4ea-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="4d4ea-218">File (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="4d4ea-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="4d4ea-219">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="4d4ea-219">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="4d4ea-220">Raccolte in memoria</span><span class="sxs-lookup"><span data-stu-id="4d4ea-220">In-memory collections</span></span> |
| <span data-ttu-id="4d4ea-221">[Segreti utente (Secret Manager)](xref:security/app-secrets) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="4d4ea-221">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="4d4ea-222">File nella directory dei profili utente</span><span class="sxs-lookup"><span data-stu-id="4d4ea-222">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="4d4ea-223">Provider</span><span class="sxs-lookup"><span data-stu-id="4d4ea-223">Provider</span></span> | <span data-ttu-id="4d4ea-224">Fornisce la configurazione da&hellip;</span><span class="sxs-lookup"><span data-stu-id="4d4ea-224">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="4d4ea-225">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="4d4ea-225">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="4d4ea-226">Parametri della riga di comando</span><span class="sxs-lookup"><span data-stu-id="4d4ea-226">Command-line parameters</span></span> |
| [<span data-ttu-id="4d4ea-227">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="4d4ea-227">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="4d4ea-228">Origine personalizzata</span><span class="sxs-lookup"><span data-stu-id="4d4ea-228">Custom source</span></span> |
| [<span data-ttu-id="4d4ea-229">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="4d4ea-229">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="4d4ea-230">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="4d4ea-230">Environment variables</span></span> |
| [<span data-ttu-id="4d4ea-231">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="4d4ea-231">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="4d4ea-232">File (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="4d4ea-232">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="4d4ea-233">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="4d4ea-233">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="4d4ea-234">Raccolte in memoria</span><span class="sxs-lookup"><span data-stu-id="4d4ea-234">In-memory collections</span></span> |
| <span data-ttu-id="4d4ea-235">[Segreti utente (Secret Manager)](xref:security/app-secrets) (argomenti *Sicurezza*)</span><span class="sxs-lookup"><span data-stu-id="4d4ea-235">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="4d4ea-236">File nella directory dei profili utente</span><span class="sxs-lookup"><span data-stu-id="4d4ea-236">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="4d4ea-237">Le origini di configurazione vengono lette nell'ordine in cui vengono specificati i rispetti provider di configurazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-237">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="4d4ea-238">I provider di configurazione descritti in questo argomento vengono presentati in ordine alfabetico e non nell'ordine in cui potrebbe disporli il codice.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-238">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="4d4ea-239">Ordinare i provider di configurazione nel codice in base alle specifiche priorità per le origini di configurazione sottostanti.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-239">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="4d4ea-240">Una sequenza tipica di provider di configurazione è:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-240">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="4d4ea-241">File (*appsettings.json*, *appsettings.{Environment}.json*, dove `{Environment}` è l'ambiente di hosting corrente dell'app)</span><span class="sxs-lookup"><span data-stu-id="4d4ea-241">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="4d4ea-242">Insieme di credenziali delle chiavi di Azure</span><span class="sxs-lookup"><span data-stu-id="4d4ea-242">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="4d4ea-243">[Segreti utente (Secret Manager)](xref:security/app-secrets) (sono nell'ambiente Development)</span><span class="sxs-lookup"><span data-stu-id="4d4ea-243">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="4d4ea-244">Variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="4d4ea-244">Environment variables</span></span>
1. <span data-ttu-id="4d4ea-245">Argomenti della riga di comando</span><span class="sxs-lookup"><span data-stu-id="4d4ea-245">Command-line arguments</span></span>

<span data-ttu-id="4d4ea-246">È pratica comune posizionare il provider di configurazione della riga di comando per ultimo in una serie di provider per consentire agli argomenti della riga di comando di sostituire la configurazione impostata da altri provider.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-246">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4d4ea-247">Questa sequenza di provider viene applicata quando si inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-247">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="4d4ea-248">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-248">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4d4ea-249">È possibile creare questa sequenza di provider per l'app (non l'host) con un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> e una chiamata al relativo metodo <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> in `Startup`:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-249">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

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

<span data-ttu-id="4d4ea-250">Nell'esempio precedente, il nome dell'ambiente (`env.EnvironmentName`) e il nome di assembly dell'app (`env.ApplicationName`) vengono forniti da <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-250">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="4d4ea-251">Per ulteriori informazioni, vedere <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-251">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="configureappconfiguration"></a><span data-ttu-id="4d4ea-252">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="4d4ea-252">ConfigureAppConfiguration</span></span>

<span data-ttu-id="4d4ea-253">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host Web per specificare i provider di configurazione dell'app, oltre a quelli aggiunti automaticamente da <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-253">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the Web Host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="4d4ea-254">Provider di configurazione della riga di comando</span><span class="sxs-lookup"><span data-stu-id="4d4ea-254">Command-line Configuration Provider</span></span>

<span data-ttu-id="4d4ea-255"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> carica la configurazione da coppie chiave-valore di argomenti della riga di comando in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-255">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="4d4ea-256">Per attivare la configurazione della riga di comando, metodo di estensione <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> viene chiamato su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-256">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4d4ea-257">`AddCommandLine` viene chiamato automaticamente quando si Inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-257">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="4d4ea-258">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-258">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="4d4ea-259">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-259">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="4d4ea-260">Una configurazione facoltativa da *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-260">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="4d4ea-261">[Segreti utente (Secret Manager)](xref:security/app-secrets) (nell'ambiente Development).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-261">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="4d4ea-262">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-262">Environment variables.</span></span>

<span data-ttu-id="4d4ea-263">`CreateDefaultBuilder` aggiunge il provider di configurazione della riga di comando per ultimo.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-263">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="4d4ea-264">Gli argomenti della riga di comando passati in fase di esecuzione sostituiscono la configurazione impostata dagli altri provider.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-264">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="4d4ea-265">`CreateDefaultBuilder` opera durante la creazione dell'host.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-265">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="4d4ea-266">Pertanto, la configurazione della riga di comando attivata da `CreateDefaultBuilder` può influire sul modo in cui l'host viene configurato.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-266">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4d4ea-267">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-267">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="4d4ea-268">`AddCommandLine` è già stato chiamato da `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-268">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="4d4ea-269">Se è necessario specificare la configurazione dell'app ed è ancora possibile eseguire l'override della configurazione con argomenti della riga di comando, chiamare i provider aggiuntivi dell'app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> e chiamare infine `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-269">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

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
                config.AddCommandLine(args)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4d4ea-270">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-270">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="4d4ea-271">Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-271">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="4d4ea-272">`AddCommandLine` è già stato chiamato da `CreateDefaultBuilder` quando è stato chiamato `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-272">`AddCommandLine` has already been called by `CreateDefaultBuilder` when `UseConfiguration` is called.</span></span> <span data-ttu-id="4d4ea-273">Se è necessario specificare la configurazione dell'app ed è ancora possibile eseguire l'override della configurazione con argomenti della riga di comando, chiamare i provider aggiuntivi dell'app in un `ConfigurationBuilder` e chiamare infine `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-273">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers on a `ConfigurationBuilder` and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="4d4ea-274">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-274">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4d4ea-275">Per attivare la configurazione della riga di comando, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-275">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4d4ea-276">Chiamare il provider per ultimo per consentire agli argomenti della riga di comando passati in fase di esecuzione di sostituire la configurazione impostata da altri provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-276">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="4d4ea-277">Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-277">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="4d4ea-278">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="4d4ea-278">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4d4ea-279">L'app di esempio 2.x consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include una chiamata a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-279">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4d4ea-280">L'app di esempio 1.x chiama <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> per un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-280">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="4d4ea-281">Aprire un prompt dei comandi nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-281">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="4d4ea-282">Fornire un argomento della riga di comando per il comando `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-282">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="4d4ea-283">Dopo che l'app è in esecuzione, aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-283">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="4d4ea-284">Notare che l'output contiene la coppia chiave-valore per l'argomento della riga di comando di configurazione fornito per `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-284">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="4d4ea-285">Argomenti</span><span class="sxs-lookup"><span data-stu-id="4d4ea-285">Arguments</span></span>

<span data-ttu-id="4d4ea-286">Il valore deve seguire un segno di uguale (`=`) o la chiave deve avere un prefisso (`--` o `/`) quando il valore segue uno spazio.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-286">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="4d4ea-287">Il valore può essere null se viene usato un segno di uguale (ad esempio, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-287">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="4d4ea-288">Prefisso chiave</span><span class="sxs-lookup"><span data-stu-id="4d4ea-288">Key prefix</span></span>               | <span data-ttu-id="4d4ea-289">Esempio</span><span class="sxs-lookup"><span data-stu-id="4d4ea-289">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="4d4ea-290">Nessun prefisso</span><span class="sxs-lookup"><span data-stu-id="4d4ea-290">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="4d4ea-291">Due trattini (`--`)</span><span class="sxs-lookup"><span data-stu-id="4d4ea-291">Two dashes (`--`)</span></span>        | <span data-ttu-id="4d4ea-292">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="4d4ea-292">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="4d4ea-293">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="4d4ea-293">Forward slash (`/`)</span></span>      | <span data-ttu-id="4d4ea-294">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="4d4ea-294">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="4d4ea-295">Nello stesso comando, non mischiare coppie chiave-valore di argomenti della riga di comando che usano un segno di uguale con coppie chiave-valore che usano uno spazio.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-295">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="4d4ea-296">Comandi di esempio:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-296">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value --CommandLineKey2=value /CommandLineKey2=value
dotnet run --CommandLineKey1 value /CommandLineKey2 value
dotnet run CommandLineKey1= CommandLineKey2=value
```

### <a name="switch-mappings"></a><span data-ttu-id="4d4ea-297">Mapping di sostituzione</span><span class="sxs-lookup"><span data-stu-id="4d4ea-297">Switch mappings</span></span>

<span data-ttu-id="4d4ea-298">I mapping di sostituzione consentono di usare la logica di sostituzione del nome della chiave.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-298">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="4d4ea-299">Quando si crea manualmente la configurazione con un <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, è possibile specificare un dizionario di mapping di sostituzione per il metodo <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-299">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="4d4ea-300">Quando viene utilizzato il dizionario dei mapping di sostituzione, nel dizionario viene controllata la presenza di una chiave corrispondente alla chiave fornita da un argomento della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-300">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="4d4ea-301">Se la chiave della riga di comando viene trovata nel dizionario, il valore del dizionario (la sostituzione della chiave) viene passato nuovamente per impostare la coppia chiave-valore nella configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-301">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="4d4ea-302">Un mapping di sostituzione è necessario per le chiavi della riga di comando con un trattino singolo (`-`) come prefisso.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-302">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="4d4ea-303">Regole principali del dizionario dei mapping di sostituzione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-303">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="4d4ea-304">Le sostituzioni devono iniziare con un trattino (`-`) o un trattino doppio (`--`).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-304">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="4d4ea-305">Il dizionario dei mapping di sostituzione non deve contenere chiavi duplicate.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-305">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4d4ea-306">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-306">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddCommandLine(args, _switchMappings)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4d4ea-307">Come illustrato nell'esempio precedente, la chiamata a `CreateDefaultBuilder` non deve passare argomenti quando vengono usati i mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-307">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="4d4ea-308">La chiamata `AddCommandLine` del metodo `CreateDefaultBuilder` non include sostituzioni mappate e non è possibile passare il dizionario dei mapping di sostituzione a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-308">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="4d4ea-309">Se gli argomenti includono una sostituzione mappata e vengono passati a `CreateDefaultBuilder`, l'inizializzazione del rispettivo provider `AddCommandLine` non riesce con <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-309">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="4d4ea-310">La soluzione consiste nel non passare gli argomenti a `CreateDefaultBuilder` consentendo invece al metodo `AddCommandLine` del metodo `ConfigurationBuilder` di elaborare entrambi gli argomenti e il dizionario dei mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-310">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="4d4ea-311">Come illustrato nell'esempio precedente, la chiamata a `CreateDefaultBuilder` non deve passare argomenti quando vengono usati i mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-311">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="4d4ea-312">La chiamata `AddCommandLine` del metodo `CreateDefaultBuilder` non include sostituzioni mappate e non è possibile passare il dizionario dei mapping di sostituzione a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-312">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="4d4ea-313">Se gli argomenti includono una sostituzione mappata e vengono passati a `CreateDefaultBuilder`, l'inizializzazione del rispettivo provider `AddCommandLine` non riesce con <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-313">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="4d4ea-314">La soluzione consiste nel non passare gli argomenti a `CreateDefaultBuilder` consentendo invece al metodo `AddCommandLine` del metodo `ConfigurationBuilder` di elaborare entrambi gli argomenti e il dizionario dei mapping di sostituzione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-314">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="4d4ea-315">Il dizionario dei mapping di sostituzione creato contiene i dati visualizzati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-315">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="4d4ea-316">Chiave</span><span class="sxs-lookup"><span data-stu-id="4d4ea-316">Key</span></span>       | <span data-ttu-id="4d4ea-317">Valore</span><span class="sxs-lookup"><span data-stu-id="4d4ea-317">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="4d4ea-318">Se le chiavi con mapping di sostituzione vengono usate all'avvio dell'app, la configurazione riceve il valore di configurazione per la chiave fornita dal dizionario:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-318">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="4d4ea-319">Dopo aver eseguito il comando precedente, la configurazione contiene i valori mostrati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-319">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="4d4ea-320">Chiave</span><span class="sxs-lookup"><span data-stu-id="4d4ea-320">Key</span></span>               | <span data-ttu-id="4d4ea-321">Valore</span><span class="sxs-lookup"><span data-stu-id="4d4ea-321">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="4d4ea-322">Provider di configurazione delle variabili di ambiente</span><span class="sxs-lookup"><span data-stu-id="4d4ea-322">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="4d4ea-323"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> carica la configurazione da coppie chiave-valore di variabili di ambiente in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-323">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="4d4ea-324">Per attivare la configurazione delle variabili di ambiente, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-324">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4d4ea-325">Quando si utilizzano chiavi gerarchiche in variabili di ambiente, il separatore due punti (`:`) potrebbe non funzionare in tutte le piattaforme.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-325">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="4d4ea-326">Il doppio carattere di sottolineatura (`__`) è supportato da tutte le piattaforme e viene sostituito con due punti.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-326">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="4d4ea-327">[Servizio App di Azure](https://azure.microsoft.com/services/app-service/) consente di impostare variabili di ambiente nel portale di Azure che possono sostituire la configurazione delle app tramite il provider di configurazione delle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-327">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="4d4ea-328">Per altre informazioni, vedere [App di Azure: Eseguire l'override della configurazione delle app usando il portale di Azure](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-328">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4d4ea-329">`AddEnvironmentVariables` viene chiamato automaticamente quando si Inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-329">`AddEnvironmentVariables` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="4d4ea-330">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-330">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="4d4ea-331">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-331">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="4d4ea-332">Una configurazione facoltativa da *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-332">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="4d4ea-333">[Segreti utente (Secret Manager)](xref:security/app-secrets) (nell'ambiente Development).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-333">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="4d4ea-334">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-334">Command-line arguments.</span></span>

<span data-ttu-id="4d4ea-335">Il provider di configurazione delle variabili di ambiente viene chiamato dopo aver stabilito la configurazione dai segreti utente e dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-335">The Environment Variable Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="4d4ea-336">La chiamata del provider in questa posizione consente alle variabili di ambiente lette in fase di esecuzione di sostituire la configurazione impostata dai segreti utente e dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-336">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4d4ea-337">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-337">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="4d4ea-338">`AddEnvironmentVariables` per variabili di ambiente con prefisso `ASPNETCORE_` è già stato chiamato da `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-338">`AddEnvironmentVariables` for environment variables prefixed with `ASPNETCORE_` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="4d4ea-339">Se è necessario specificare la configurazione dell'app da variabili di ambiente aggiuntive, chiamare i provider aggiuntivi dell'app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> e chiamare `AddEnvironmentVariables` con il prefisso.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-339">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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
                config.AddEnvironmentVariables(prefix: "PREFIX_")
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4d4ea-340">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-340">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="4d4ea-341">Chiamare il metodo di estensione `AddEnvironmentVariables` su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-341">Call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="4d4ea-342">Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-342">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="4d4ea-343">`AddEnvironmentVariables` per variabili di ambiente con prefisso `ASPNETCORE_` è già stato chiamato da `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-343">`AddEnvironmentVariables` for environment variables prefixed with `ASPNETCORE_` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="4d4ea-344">Se è necessario specificare la configurazione dell'app da variabili di ambiente aggiuntive, chiamare i provider aggiuntivi dell'app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> e chiamare `AddEnvironmentVariables` con il prefisso.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-344">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="4d4ea-345">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-345">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4d4ea-346">Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-346">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="4d4ea-347">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="4d4ea-347">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4d4ea-348">L'app di esempio 2.x consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include una chiamata a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-348">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4d4ea-349">L'app di esempio 1.x chiama `AddEnvironmentVariables` per un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-349">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="4d4ea-350">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-350">Run the sample app.</span></span> <span data-ttu-id="4d4ea-351">Aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-351">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="4d4ea-352">Notare che l'output contiene la coppia chiave-valore per la variabile di ambiente `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-352">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="4d4ea-353">Il valore riflette l'ambiente in cui l'app è in esecuzione, in genere `Development` durante l'esecuzione locale.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-353">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="4d4ea-354">Per mantenere breve l'elenco delle variabili di ambiente restituito dall'app, l'app filtra le variabili di ambiente includendo quelle che iniziano come segue:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-354">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="4d4ea-355">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="4d4ea-355">ASPNETCORE_</span></span>
* <span data-ttu-id="4d4ea-356">urls</span><span class="sxs-lookup"><span data-stu-id="4d4ea-356">urls</span></span>
* <span data-ttu-id="4d4ea-357">Registrazione</span><span class="sxs-lookup"><span data-stu-id="4d4ea-357">Logging</span></span>
* <span data-ttu-id="4d4ea-358">ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="4d4ea-358">ENVIRONMENT</span></span>
* <span data-ttu-id="4d4ea-359">contentRoot</span><span class="sxs-lookup"><span data-stu-id="4d4ea-359">contentRoot</span></span>
* <span data-ttu-id="4d4ea-360">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="4d4ea-360">AllowedHosts</span></span>
* <span data-ttu-id="4d4ea-361">applicationName</span><span class="sxs-lookup"><span data-stu-id="4d4ea-361">applicationName</span></span>
* <span data-ttu-id="4d4ea-362">CommandLine</span><span class="sxs-lookup"><span data-stu-id="4d4ea-362">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4d4ea-363">Se si desidera esporre tutte le variabili di ambiente disponibili all'app, modificare `FilteredConfiguration` in *Pages/Index.cshtml.cs* come segue:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-363">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4d4ea-364">Se si desidera esporre tutte le variabili di ambiente disponibili all'app, modificare `FilteredConfiguration` in *Controllers/HomeController.cs* come segue:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-364">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="4d4ea-365">Prefissi</span><span class="sxs-lookup"><span data-stu-id="4d4ea-365">Prefixes</span></span>

<span data-ttu-id="4d4ea-366">Le variabili di ambiente caricate nella configurazione dell'app vengono filtrate quando si specifica un prefisso per il metodo `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-366">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="4d4ea-367">Ad esempio, per filtrare le variabili di ambiente in base al prefisso `CUSTOM_`, fornire il prefisso al provider di configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-367">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="4d4ea-368">Il prefisso viene rimosso quando vengono create le coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-368">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4d4ea-369">Il metodo di servizio statico `CreateDefaultBuilder` crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> per stabilire l'host dell'app.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-369">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="4d4ea-370">Quando viene creato `WebHostBuilder`, la configurazione dell'host corrispondente viene individuata nelle variabili di ambiente con il prefisso `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-370">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="4d4ea-371">**Prefissi della stringa di connessione**</span><span class="sxs-lookup"><span data-stu-id="4d4ea-371">**Connection string prefixes**</span></span>

<span data-ttu-id="4d4ea-372">L'API di configurazione prevede regole di elaborazione speciali per quattro variabili di ambiente di stringa di connessione coinvolte nella configurazione di stringhe di connessione di Azure per l'ambiente dell'app.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-372">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="4d4ea-373">Le variabili di ambiente con i prefissi indicati nella tabella vengono caricate nell'app se non viene fornito alcun prefisso a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-373">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="4d4ea-374">Prefisso della stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="4d4ea-374">Connection string prefix</span></span> | <span data-ttu-id="4d4ea-375">Provider</span><span class="sxs-lookup"><span data-stu-id="4d4ea-375">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="4d4ea-376">Provider personalizzato</span><span class="sxs-lookup"><span data-stu-id="4d4ea-376">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="4d4ea-377">MySQL</span><span class="sxs-lookup"><span data-stu-id="4d4ea-377">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="4d4ea-378">Database SQL di Azure</span><span class="sxs-lookup"><span data-stu-id="4d4ea-378">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="4d4ea-379">SQL Server</span><span class="sxs-lookup"><span data-stu-id="4d4ea-379">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="4d4ea-380">Quando una variabile di ambiente viene individuata e caricata nella configurazione con uno qualsiasi dei quattro prefissi indicati nella tabella:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-380">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="4d4ea-381">La chiave di configurazione viene creata rimuovendo il prefisso della variabile di ambiente e aggiungendo una sezione per la chiave di configurazione (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-381">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="4d4ea-382">Viene creata una nuova coppia chiave-valore della configurazione che rappresenta il provider di connessione al database (ad eccezione di `CUSTOMCONNSTR_`, che non ha un provider dichiarato).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-382">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="4d4ea-383">Chiave della variabile di ambiente</span><span class="sxs-lookup"><span data-stu-id="4d4ea-383">Environment variable key</span></span> | <span data-ttu-id="4d4ea-384">Chiave di configurazione convertita</span><span class="sxs-lookup"><span data-stu-id="4d4ea-384">Converted configuration key</span></span> | <span data-ttu-id="4d4ea-385">Voce di configurazione del provider</span><span class="sxs-lookup"><span data-stu-id="4d4ea-385">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="4d4ea-386">Voce di configurazione non creata.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-386">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="4d4ea-387">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-387">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="4d4ea-388">Valore: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="4d4ea-388">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="4d4ea-389">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-389">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="4d4ea-390">Valore: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="4d4ea-390">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="4d4ea-391">Chiave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-391">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="4d4ea-392">Valore: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="4d4ea-392">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="4d4ea-393">Provider di configurazione dei file</span><span class="sxs-lookup"><span data-stu-id="4d4ea-393">File Configuration Provider</span></span>

<span data-ttu-id="4d4ea-394"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> è la classe base per il caricamento della configurazione dal file system.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-394"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="4d4ea-395">I provider di configurazione seguenti sono dedicati per specifici tipi di file:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-395">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="4d4ea-396">Provider di configurazione INI</span><span class="sxs-lookup"><span data-stu-id="4d4ea-396">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="4d4ea-397">Provider di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="4d4ea-397">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="4d4ea-398">Provider di configurazione XML</span><span class="sxs-lookup"><span data-stu-id="4d4ea-398">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="4d4ea-399">Provider di configurazione INI</span><span class="sxs-lookup"><span data-stu-id="4d4ea-399">INI Configuration Provider</span></span>

<span data-ttu-id="4d4ea-400"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> carica la configurazione da coppie chiave-valore di file INI in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-400">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="4d4ea-401">Per attivare la configurazione di file INI, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-401">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4d4ea-402">I due punti possono essere usati come delimitatore di sezione nella configurazione di file INI.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-402">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="4d4ea-403">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-403">Overloads permit specifying:</span></span>

* <span data-ttu-id="4d4ea-404">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-404">Whether the file is optional.</span></span>
* <span data-ttu-id="4d4ea-405">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-405">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="4d4ea-406">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-406">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4d4ea-407">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-407">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddIniFile("config.ini", optional: true, reloadOnChange: true)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4d4ea-408">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-408">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="4d4ea-409">Quando si chiama `CreateDefaultBuilder`, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-409">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="4d4ea-410">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-410">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4d4ea-411">Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-411">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="4d4ea-412">Un esempio generico di un file di configurazione INI:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-412">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="4d4ea-413">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-413">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="4d4ea-414">section0:key0</span><span class="sxs-lookup"><span data-stu-id="4d4ea-414">section0:key0</span></span>
* <span data-ttu-id="4d4ea-415">section0:key1</span><span class="sxs-lookup"><span data-stu-id="4d4ea-415">section0:key1</span></span>
* <span data-ttu-id="4d4ea-416">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="4d4ea-416">section1:subsection:key</span></span>
* <span data-ttu-id="4d4ea-417">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="4d4ea-417">section2:subsection0:key</span></span>
* <span data-ttu-id="4d4ea-418">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="4d4ea-418">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="4d4ea-419">Provider di configurazione JSON</span><span class="sxs-lookup"><span data-stu-id="4d4ea-419">JSON Configuration Provider</span></span>

<span data-ttu-id="4d4ea-420">Il <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> carica la configurazione da coppie chiave-valore di file JSON in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-420">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="4d4ea-421">Per attivare la configurazione di file JSON, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-421">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4d4ea-422">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-422">Overloads permit specifying:</span></span>

* <span data-ttu-id="4d4ea-423">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-423">Whether the file is optional.</span></span>
* <span data-ttu-id="4d4ea-424">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-424">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="4d4ea-425">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-425">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4d4ea-426">`AddJsonFile` viene chiamato automaticamente due volte quando si inizializza un nuovo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-426">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="4d4ea-427">Il metodo viene chiamato per caricare la configurazione da:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-427">The method is called to load configuration from:</span></span>

* <span data-ttu-id="4d4ea-428">*appsettings.json* &ndash; Questo file viene letto per primo.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-428">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="4d4ea-429">La versione dell'ambiente del file può sostituire i valori forniti dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-429">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="4d4ea-430">*appsettings.{Environment}.json* &ndash; La versione dell'ambiente del file viene caricata in base a [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-430">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="4d4ea-431">Per altre informazioni, vedere [Host Web: Configurare un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-431">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="4d4ea-432">`CreateDefaultBuilder` carica anche:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-432">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="4d4ea-433">Variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-433">Environment variables.</span></span>
* <span data-ttu-id="4d4ea-434">[Segreti utente (Secret Manager)](xref:security/app-secrets) (nell'ambiente Development).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-434">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="4d4ea-435">Argomenti della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-435">Command-line arguments.</span></span>

<span data-ttu-id="4d4ea-436">Il provider di configurazione JSON viene stabilito per primo.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-436">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="4d4ea-437">I segreti utente, le variabili di ambiente e gli argomenti della riga di comando sostituiscono quindi la configurazione impostata dai file *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-437">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4d4ea-438">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app per file diversi da *appsettings.json* e *appsettings.{Environment}.json*:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-438">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

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
                config.AddJsonFile("config.json", optional: true, reloadOnChange: true)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4d4ea-439">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-439">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="4d4ea-440">È possibile chiamare direttamente il metodo di estensione `AddJsonFile` su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-440">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4d4ea-441">Quando si chiama `CreateDefaultBuilder`, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-441">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="4d4ea-442">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-442">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4d4ea-443">Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-443">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="4d4ea-444">**Esempio**</span><span class="sxs-lookup"><span data-stu-id="4d4ea-444">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4d4ea-445">L'app di esempio 2.x consente di sfruttare il metodo di servizio statico `CreateDefaultBuilder` per creare l'host, che include due chiamate a `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-445">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="4d4ea-446">La configurazione viene caricata da *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-446">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4d4ea-447">L'app di esempio 1.x chiama `AddJsonFile` due volte su un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-447">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="4d4ea-448">La configurazione viene caricata da *appsettings.json* e *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-448">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="4d4ea-449">Eseguire l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-449">Run the sample app.</span></span> <span data-ttu-id="4d4ea-450">Aprire un browser per l'app all'indirizzo `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-450">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="4d4ea-451">Notare che l'output contiene coppie chiave-valore per la configurazione mostrata nella tabella in base all'ambiente.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-451">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="4d4ea-452">Le chiavi di configurazione di registrazione usano i due punti (`:`) come separatore gerarchico.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-452">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="4d4ea-453">Chiave</span><span class="sxs-lookup"><span data-stu-id="4d4ea-453">Key</span></span>                        | <span data-ttu-id="4d4ea-454">Valore Development</span><span class="sxs-lookup"><span data-stu-id="4d4ea-454">Development Value</span></span> | <span data-ttu-id="4d4ea-455">Valore Production</span><span class="sxs-lookup"><span data-stu-id="4d4ea-455">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="4d4ea-456">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="4d4ea-456">Logging:LogLevel:System</span></span>    | <span data-ttu-id="4d4ea-457">Informazioni</span><span class="sxs-lookup"><span data-stu-id="4d4ea-457">Information</span></span>       | <span data-ttu-id="4d4ea-458">Informazioni</span><span class="sxs-lookup"><span data-stu-id="4d4ea-458">Information</span></span>      |
| <span data-ttu-id="4d4ea-459">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="4d4ea-459">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="4d4ea-460">Informazioni</span><span class="sxs-lookup"><span data-stu-id="4d4ea-460">Information</span></span>       | <span data-ttu-id="4d4ea-461">Informazioni</span><span class="sxs-lookup"><span data-stu-id="4d4ea-461">Information</span></span>      |
| <span data-ttu-id="4d4ea-462">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="4d4ea-462">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="4d4ea-463">Debug</span><span class="sxs-lookup"><span data-stu-id="4d4ea-463">Debug</span></span>             | <span data-ttu-id="4d4ea-464">Error</span><span class="sxs-lookup"><span data-stu-id="4d4ea-464">Error</span></span>            |
| <span data-ttu-id="4d4ea-465">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="4d4ea-465">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="4d4ea-466">Provider di configurazione XML</span><span class="sxs-lookup"><span data-stu-id="4d4ea-466">XML Configuration Provider</span></span>

<span data-ttu-id="4d4ea-467">Il <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> carica la configurazione da coppie chiave-valore di file XML in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-467">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="4d4ea-468">Per attivare la configurazione di file XML, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-468">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4d4ea-469">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-469">Overloads permit specifying:</span></span>

* <span data-ttu-id="4d4ea-470">Se il file è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-470">Whether the file is optional.</span></span>
* <span data-ttu-id="4d4ea-471">Se la configurazione viene ricaricata se viene modificato il file.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-471">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="4d4ea-472">Il <xref:Microsoft.Extensions.FileProviders.IFileProvider> usato per accedere al file.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-472">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="4d4ea-473">Il nodo radice del file di configurazione viene ignorato quando vengono create le coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-473">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="4d4ea-474">Non specificare una definizione DTD (Document Type Definition) o uno spazio dei nomi nel file.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-474">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4d4ea-475">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-475">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddXmlFile("config.xml", optional: true, reloadOnChange: true)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4d4ea-476">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-476">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="4d4ea-477">Quando si chiama `CreateDefaultBuilder`, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-477">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="4d4ea-478">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-478">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4d4ea-479">Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-479">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="4d4ea-480">I file di configurazione XML possono usare nomi di elementi distinti per le sezioni ripetute:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-480">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="4d4ea-481">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-481">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="4d4ea-482">section0:key0</span><span class="sxs-lookup"><span data-stu-id="4d4ea-482">section0:key0</span></span>
* <span data-ttu-id="4d4ea-483">section0:key1</span><span class="sxs-lookup"><span data-stu-id="4d4ea-483">section0:key1</span></span>
* <span data-ttu-id="4d4ea-484">section1:key0</span><span class="sxs-lookup"><span data-stu-id="4d4ea-484">section1:key0</span></span>
* <span data-ttu-id="4d4ea-485">section1:key1</span><span class="sxs-lookup"><span data-stu-id="4d4ea-485">section1:key1</span></span>

<span data-ttu-id="4d4ea-486">La ripetizione di elementi che usano lo stesso nome di elemento funziona se si usa l'attributo `name` per distinguere gli elementi:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-486">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="4d4ea-487">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-487">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="4d4ea-488">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="4d4ea-488">section:section0:key:key0</span></span>
* <span data-ttu-id="4d4ea-489">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="4d4ea-489">section:section0:key:key1</span></span>
* <span data-ttu-id="4d4ea-490">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="4d4ea-490">section:section1:key:key0</span></span>
* <span data-ttu-id="4d4ea-491">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="4d4ea-491">section:section1:key:key1</span></span>

<span data-ttu-id="4d4ea-492">È possibile usare attributi per fornire valori:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-492">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="4d4ea-493">Il file di configurazione precedente carica le chiavi seguenti con `value`:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-493">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="4d4ea-494">key:attribute</span><span class="sxs-lookup"><span data-stu-id="4d4ea-494">key:attribute</span></span>
* <span data-ttu-id="4d4ea-495">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="4d4ea-495">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="4d4ea-496">Provider di configurazione KeyPerFile</span><span class="sxs-lookup"><span data-stu-id="4d4ea-496">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="4d4ea-497">Il <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> usa i file di una directory come coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-497">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="4d4ea-498">La chiave è il nome del file.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-498">The key is the file name.</span></span> <span data-ttu-id="4d4ea-499">Il valore contiene il contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-499">The value contains the file's contents.</span></span> <span data-ttu-id="4d4ea-500">Il provider di configurazione KeyPerFile viene usato in scenari di hosting Docker.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-500">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="4d4ea-501">Per attivare la configurazione chiave-per-file, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-501">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="4d4ea-502">Il `directoryPath` per i file deve essere un percorso assoluto.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-502">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="4d4ea-503">Gli overload consentono di specificare:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-503">Overloads permit specifying:</span></span>

* <span data-ttu-id="4d4ea-504">Un delegato `Action<KeyPerFileConfigurationSource>` che configura l'origine.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-504">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="4d4ea-505">Se la directory è facoltativa e il percorso della directory.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-505">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="4d4ea-506">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-506">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddKeyPerFile(directoryPath: path, optional: true)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4d4ea-507">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-507">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="4d4ea-508">Provider di configurazione della memoria</span><span class="sxs-lookup"><span data-stu-id="4d4ea-508">Memory Configuration Provider</span></span>

<span data-ttu-id="4d4ea-509">Il <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> usa una raccolta in memoria come coppie chiave-valore della configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-509">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="4d4ea-510">Per attivare la configurazione della raccolta in memoria, chiamare il metodo di estensione <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> su un'istanza di <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-510">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="4d4ea-511">Il provider di configurazione può essere inizializzato con un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-511">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4d4ea-512">Chiamare <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> quando si crea l'host per specificare la configurazione dell'app:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-512">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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
                config.AddInMemoryCollection(_dict)
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="4d4ea-513">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-513">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="4d4ea-514">Quando si chiama `CreateDefaultBuilder`, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-514">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="4d4ea-515">Quando si crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> direttamente, chiamare <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-515">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4d4ea-516">Applicare la configurazione a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con il metodo <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-516">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="4d4ea-517">GetValue</span><span class="sxs-lookup"><span data-stu-id="4d4ea-517">GetValue</span></span>

<span data-ttu-id="4d4ea-518">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) estrae un valore dalla configurazione con una chiave specificata e lo converte nel tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-518">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="4d4ea-519">Un overload consente di specificare un valore predefinito se non viene trovata la chiave.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-519">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="4d4ea-520">L'esempio seguente estrae il valore della stringa dalla configurazione con la chiave `NumberKey`, assegna al valore il tipo `int` e archivia il valore nella variabile `intValue`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-520">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="4d4ea-521">Se `NumberKey` non viene trovato nelle chiavi di configurazione `intValue` riceve il valore predefinito `99`:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-521">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="4d4ea-522">GetSection, GetChildren ed Exists</span><span class="sxs-lookup"><span data-stu-id="4d4ea-522">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="4d4ea-523">Per gli esempi che seguono, prendere in considerazione il file JSON seguente.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-523">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="4d4ea-524">Vengono trovate quattro chiavi in due sezioni, una delle quali include una coppia di sottosezioni:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-524">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="4d4ea-525">Quando il file viene letto nella configurazione, vengono create le chiavi gerarchiche univoche seguenti per contenere i valori di configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-525">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="4d4ea-526">section0:key0</span><span class="sxs-lookup"><span data-stu-id="4d4ea-526">section0:key0</span></span>
* <span data-ttu-id="4d4ea-527">section0:key1</span><span class="sxs-lookup"><span data-stu-id="4d4ea-527">section0:key1</span></span>
* <span data-ttu-id="4d4ea-528">section1:key0</span><span class="sxs-lookup"><span data-stu-id="4d4ea-528">section1:key0</span></span>
* <span data-ttu-id="4d4ea-529">section1:key1</span><span class="sxs-lookup"><span data-stu-id="4d4ea-529">section1:key1</span></span>
* <span data-ttu-id="4d4ea-530">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="4d4ea-530">section2:subsection0:key0</span></span>
* <span data-ttu-id="4d4ea-531">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="4d4ea-531">section2:subsection0:key1</span></span>
* <span data-ttu-id="4d4ea-532">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="4d4ea-532">section2:subsection1:key0</span></span>
* <span data-ttu-id="4d4ea-533">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="4d4ea-533">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="4d4ea-534">GetSection</span><span class="sxs-lookup"><span data-stu-id="4d4ea-534">GetSection</span></span>

<span data-ttu-id="4d4ea-535">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) estrae una sottosezione della configurazione con la chiave di sottosezione specificata.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-535">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="4d4ea-536">Per restituire una <xref:Microsoft.Extensions.Configuration.IConfigurationSection> che contiene solo le coppie chiave-valore in `section1`, chiamare `GetSection` e fornire il nome della sezione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-536">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="4d4ea-537">In modo analogo, per ottenere i valori per le chiavi in `section2:subsection0`, chiamare `GetSection` e fornire il percorso della sezione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-537">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="4d4ea-538">`GetSection` non restituisce mai `null`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-538">`GetSection` never returns `null`.</span></span> <span data-ttu-id="4d4ea-539">Se non viene trovata una sezione corrispondente, viene restituita una `IConfigurationSection` vuota.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-539">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

### <a name="getchildren"></a><span data-ttu-id="4d4ea-540">GetChildren</span><span class="sxs-lookup"><span data-stu-id="4d4ea-540">GetChildren</span></span>

<span data-ttu-id="4d4ea-541">Una chiamata a [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) per `section2` ottiene una `IEnumerable<IConfigurationSection>` che include:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-541">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="4d4ea-542">Esistente</span><span class="sxs-lookup"><span data-stu-id="4d4ea-542">Exists</span></span>

<span data-ttu-id="4d4ea-543">Usare [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) per determinare se esiste una sezione di configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-543">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="4d4ea-544">Considerati i dati di esempio, `sectionExists` è `false` perché non esiste una sezione `section2:subsection2` nei dati di configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-544">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="4d4ea-545">Eseguire l'associazione a una classe</span><span class="sxs-lookup"><span data-stu-id="4d4ea-545">Bind to a class</span></span>

<span data-ttu-id="4d4ea-546">La configurazione può essere associata alle classi che rappresentano gruppi di impostazioni correlate usando il *modello di opzioni*.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-546">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="4d4ea-547">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-547">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="4d4ea-548">I valori di configurazione vengono restituiti come stringhe, ma la chiamata di <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> consente la costruzione di oggetti [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-548">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="4d4ea-549">L'app di esempio contiene un modello `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="4d4ea-549">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="4d4ea-550">La sezione `starship` del file *starship.json* crea la configurazione quando l'app di esempio usa il provider di configurazione JSON per caricare la configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-550">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="4d4ea-551">Vengono create le coppie chiave-valore della configurazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-551">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="4d4ea-552">Chiave</span><span class="sxs-lookup"><span data-stu-id="4d4ea-552">Key</span></span>                   | <span data-ttu-id="4d4ea-553">Valore</span><span class="sxs-lookup"><span data-stu-id="4d4ea-553">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="4d4ea-554">starship:name</span><span class="sxs-lookup"><span data-stu-id="4d4ea-554">starship:name</span></span>         | <span data-ttu-id="4d4ea-555">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="4d4ea-555">USS Enterprise</span></span>                                    |
| <span data-ttu-id="4d4ea-556">starship:registry</span><span class="sxs-lookup"><span data-stu-id="4d4ea-556">starship:registry</span></span>     | <span data-ttu-id="4d4ea-557">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="4d4ea-557">NCC-1701</span></span>                                          |
| <span data-ttu-id="4d4ea-558">starship:class</span><span class="sxs-lookup"><span data-stu-id="4d4ea-558">starship:class</span></span>        | <span data-ttu-id="4d4ea-559">Constitution</span><span class="sxs-lookup"><span data-stu-id="4d4ea-559">Constitution</span></span>                                      |
| <span data-ttu-id="4d4ea-560">starship:length</span><span class="sxs-lookup"><span data-stu-id="4d4ea-560">starship:length</span></span>       | <span data-ttu-id="4d4ea-561">304.8</span><span class="sxs-lookup"><span data-stu-id="4d4ea-561">304.8</span></span>                                             |
| <span data-ttu-id="4d4ea-562">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="4d4ea-562">starship:commissioned</span></span> | <span data-ttu-id="4d4ea-563">False</span><span class="sxs-lookup"><span data-stu-id="4d4ea-563">False</span></span>                                             |
| <span data-ttu-id="4d4ea-564">trademark</span><span class="sxs-lookup"><span data-stu-id="4d4ea-564">trademark</span></span>             | <span data-ttu-id="4d4ea-565">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="4d4ea-565">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="4d4ea-566">L'app di esempio chiama `GetSection` con la chiave `starship`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-566">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="4d4ea-567">Le coppie chiave-valore `starship` sono isolate.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-567">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="4d4ea-568">Il metodo `Bind` viene chiamato sulla sottosezione passando un'istanza della classe `Starship`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-568">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="4d4ea-569">Dopo aver associato i valori dell'istanza, l'istanza viene assegnata a una proprietà per il rendering:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-569">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="4d4ea-570">Associazione a un oggetto grafico</span><span class="sxs-lookup"><span data-stu-id="4d4ea-570">Bind to an object graph</span></span>

<span data-ttu-id="4d4ea-571"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> è in grado di associare un intero oggetto grafico POCO.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-571"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="4d4ea-572">L'esempio contiene un modello `TvShow` il cui oggetto grafico include `Metadata` e le classi `Actors` (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="4d4ea-572">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="4d4ea-573">L'app di esempio ha un file *tvshow.xml* che contiene i dati di configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-573">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="4d4ea-574">La configurazione viene associata all'intero oggetto grafico `TvShow` con il metodo `Bind`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-574">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="4d4ea-575">L'istanza associata viene assegnata a una proprietà per il rendering:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-575">The bound instance is assigned to a property for rendering:</span></span>

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

<span data-ttu-id="4d4ea-576">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) associa e restituisce il tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-576">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="4d4ea-577">`Get<T>` può essere più comodo che usare `Bind`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-577">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="4d4ea-578">Il codice seguente mostra come usare `Get<T>` con l'esempio precedente, che consente di assegnare l'istanza associata direttamente alla proprietà usata per il rendering:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-578">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="4d4ea-579">Associare una matrice a una classe</span><span class="sxs-lookup"><span data-stu-id="4d4ea-579">Bind an array to a class</span></span>

<span data-ttu-id="4d4ea-580">*L'app di esempio dimostra i concetti spiegati in questa sezione.*</span><span class="sxs-lookup"><span data-stu-id="4d4ea-580">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="4d4ea-581">Il <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supporta l'associazione di matrici agli oggetti usando gli indici delle matrici nelle chiavi di configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-581">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="4d4ea-582">Qualsiasi formato di matrice che espone un segmento chiave numerico (`:0:`, `:1:`, &hellip; `:{n}:`) supporta l'associazione di matrici a una matrice di classi POCO.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-582">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="4d4ea-583">L'associazione viene fornita per convenzione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-583">Binding is provided by convention.</span></span> <span data-ttu-id="4d4ea-584">I provider di configurazione personalizzati non devono implementare l'associazione di matrici.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-584">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="4d4ea-585">**Elaborazione di matrici in memoria**</span><span class="sxs-lookup"><span data-stu-id="4d4ea-585">**In-memory array processing**</span></span>

<span data-ttu-id="4d4ea-586">Prendere in considerazione le chiavi di configurazione e i valori indicati nella tabella seguente.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-586">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="4d4ea-587">Chiave</span><span class="sxs-lookup"><span data-stu-id="4d4ea-587">Key</span></span>     | <span data-ttu-id="4d4ea-588">Valore</span><span class="sxs-lookup"><span data-stu-id="4d4ea-588">Value</span></span>  |
| :-----: | :----: |
| <span data-ttu-id="4d4ea-589">array:0</span><span class="sxs-lookup"><span data-stu-id="4d4ea-589">array:0</span></span> | <span data-ttu-id="4d4ea-590">value0</span><span class="sxs-lookup"><span data-stu-id="4d4ea-590">value0</span></span> |
| <span data-ttu-id="4d4ea-591">array:1</span><span class="sxs-lookup"><span data-stu-id="4d4ea-591">array:1</span></span> | <span data-ttu-id="4d4ea-592">value1</span><span class="sxs-lookup"><span data-stu-id="4d4ea-592">value1</span></span> |
| <span data-ttu-id="4d4ea-593">array:2</span><span class="sxs-lookup"><span data-stu-id="4d4ea-593">array:2</span></span> | <span data-ttu-id="4d4ea-594">value2</span><span class="sxs-lookup"><span data-stu-id="4d4ea-594">value2</span></span> |
| <span data-ttu-id="4d4ea-595">array:4</span><span class="sxs-lookup"><span data-stu-id="4d4ea-595">array:4</span></span> | <span data-ttu-id="4d4ea-596">value4</span><span class="sxs-lookup"><span data-stu-id="4d4ea-596">value4</span></span> |
| <span data-ttu-id="4d4ea-597">array:5</span><span class="sxs-lookup"><span data-stu-id="4d4ea-597">array:5</span></span> | <span data-ttu-id="4d4ea-598">value5</span><span class="sxs-lookup"><span data-stu-id="4d4ea-598">value5</span></span> |

<span data-ttu-id="4d4ea-599">Queste chiavi e i valori vengono caricati nell'app di esempio usando il provider di configurazione della memoria:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-599">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="4d4ea-600">La matrice ignora un valore per l'indice &num;3.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-600">The array skips a value for index &num;3.</span></span> <span data-ttu-id="4d4ea-601">Lo strumento di associazione di configurazione non è in grado di associare valori null o di creare voci null negli oggetti associati, situazione che diventerà chiara tra poco quando viene illustrato il risultato dell'associazione di questa matrice a un oggetto.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-601">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="4d4ea-602">Nell'app di esempio, è disponibile una classe POCO per i dati di configurazione associati:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-602">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="4d4ea-603">I dati di configurazione sono associati all'oggetto:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-603">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="4d4ea-604">È possibile usare anche la sintassi [ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*), che produce codice più compatto:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-604">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="4d4ea-605">L'oggetto associato, un'istanza di `ArrayExample`, riceve i dati di matrice dalla configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-605">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="4d4ea-606">Indice di `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="4d4ea-606">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="4d4ea-607">Valore di `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="4d4ea-607">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="4d4ea-608">0</span><span class="sxs-lookup"><span data-stu-id="4d4ea-608">0</span></span>                             | <span data-ttu-id="4d4ea-609">value0</span><span class="sxs-lookup"><span data-stu-id="4d4ea-609">value0</span></span>                        |
| <span data-ttu-id="4d4ea-610">1</span><span class="sxs-lookup"><span data-stu-id="4d4ea-610">1</span></span>                             | <span data-ttu-id="4d4ea-611">value1</span><span class="sxs-lookup"><span data-stu-id="4d4ea-611">value1</span></span>                        |
| <span data-ttu-id="4d4ea-612">2</span><span class="sxs-lookup"><span data-stu-id="4d4ea-612">2</span></span>                             | <span data-ttu-id="4d4ea-613">value2</span><span class="sxs-lookup"><span data-stu-id="4d4ea-613">value2</span></span>                        |
| <span data-ttu-id="4d4ea-614">3</span><span class="sxs-lookup"><span data-stu-id="4d4ea-614">3</span></span>                             | <span data-ttu-id="4d4ea-615">value4</span><span class="sxs-lookup"><span data-stu-id="4d4ea-615">value4</span></span>                        |
| <span data-ttu-id="4d4ea-616">4</span><span class="sxs-lookup"><span data-stu-id="4d4ea-616">4</span></span>                             | <span data-ttu-id="4d4ea-617">value5</span><span class="sxs-lookup"><span data-stu-id="4d4ea-617">value5</span></span>                        |

<span data-ttu-id="4d4ea-618">L'indice &num;3 nell'oggetto associato contiene i dati di configurazione per la chiave di configurazione `array:4` e il relativo valore `value4`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-618">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="4d4ea-619">Quando i dati di configurazione contenenti una matrice vengono associati, gli indici di matrice nelle chiavi di configurazione vengono usati semplicemente per scorrere i dati di configurazione quando si crea l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-619">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="4d4ea-620">Un valore null non può essere mantenuto nei dati di configurazione e una voce con valore null non viene creata in un oggetto associato quando una matrice nelle chiavi di configurazione ignora uno o più indici.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-620">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="4d4ea-621">L'elemento di configurazione mancante per l'indice &num;3 può essere fornito prima dell'associazione all'istanza `ArrayExamples` da qualsiasi provider di configurazione che produce la coppia chiave-valore corretta nella configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-621">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExamples` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="4d4ea-622">Se il codice di esempio include un provider di configurazione JSON aggiuntivo con la coppia chiave-valore mancante, `ArrayExamples.Entries` corrisponde alla matrice di configurazione completa:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-622">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExamples.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="4d4ea-623">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-623">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4d4ea-624">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-624">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4d4ea-625">Nel costruttore `Startup`:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-625">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="4d4ea-626">La coppia chiave-valore mostrata nella tabella viene caricata nella configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-626">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="4d4ea-627">Chiave</span><span class="sxs-lookup"><span data-stu-id="4d4ea-627">Key</span></span>             | <span data-ttu-id="4d4ea-628">Valore</span><span class="sxs-lookup"><span data-stu-id="4d4ea-628">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="4d4ea-629">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="4d4ea-629">array:entries:3</span></span> | <span data-ttu-id="4d4ea-630">value3</span><span class="sxs-lookup"><span data-stu-id="4d4ea-630">value3</span></span> |

<span data-ttu-id="4d4ea-631">Se l'istanza della classe `ArrayExamples` viene associata dopo che il provider di configurazione JSON include la voce per l'indice &num;3, la matrice `ArrayExamples.Entries` include il valore.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-631">If the `ArrayExamples` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExamples.Entries` array includes the value.</span></span>

| <span data-ttu-id="4d4ea-632">Indice di `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="4d4ea-632">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="4d4ea-633">Valore di `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="4d4ea-633">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="4d4ea-634">0</span><span class="sxs-lookup"><span data-stu-id="4d4ea-634">0</span></span>                             | <span data-ttu-id="4d4ea-635">value0</span><span class="sxs-lookup"><span data-stu-id="4d4ea-635">value0</span></span>                        |
| <span data-ttu-id="4d4ea-636">1</span><span class="sxs-lookup"><span data-stu-id="4d4ea-636">1</span></span>                             | <span data-ttu-id="4d4ea-637">value1</span><span class="sxs-lookup"><span data-stu-id="4d4ea-637">value1</span></span>                        |
| <span data-ttu-id="4d4ea-638">2</span><span class="sxs-lookup"><span data-stu-id="4d4ea-638">2</span></span>                             | <span data-ttu-id="4d4ea-639">value2</span><span class="sxs-lookup"><span data-stu-id="4d4ea-639">value2</span></span>                        |
| <span data-ttu-id="4d4ea-640">3</span><span class="sxs-lookup"><span data-stu-id="4d4ea-640">3</span></span>                             | <span data-ttu-id="4d4ea-641">value3</span><span class="sxs-lookup"><span data-stu-id="4d4ea-641">value3</span></span>                        |
| <span data-ttu-id="4d4ea-642">4</span><span class="sxs-lookup"><span data-stu-id="4d4ea-642">4</span></span>                             | <span data-ttu-id="4d4ea-643">value4</span><span class="sxs-lookup"><span data-stu-id="4d4ea-643">value4</span></span>                        |
| <span data-ttu-id="4d4ea-644">5</span><span class="sxs-lookup"><span data-stu-id="4d4ea-644">5</span></span>                             | <span data-ttu-id="4d4ea-645">value5</span><span class="sxs-lookup"><span data-stu-id="4d4ea-645">value5</span></span>                        |

<span data-ttu-id="4d4ea-646">**Elaborazione di matrice JSON**</span><span class="sxs-lookup"><span data-stu-id="4d4ea-646">**JSON array processing**</span></span>

<span data-ttu-id="4d4ea-647">Se un file JSON contiene una matrice, vengono create chiavi di configurazione per gli elementi della matrice con un indice di sezione in base zero.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-647">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="4d4ea-648">Nel file di configurazione seguente, `subsection` è una matrice:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-648">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="4d4ea-649">Il provider di configurazione JSON legge i dati di configurazione nelle coppie chiave-valore seguenti:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-649">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="4d4ea-650">Chiave</span><span class="sxs-lookup"><span data-stu-id="4d4ea-650">Key</span></span>                     | <span data-ttu-id="4d4ea-651">Valore</span><span class="sxs-lookup"><span data-stu-id="4d4ea-651">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="4d4ea-652">json_array:key</span><span class="sxs-lookup"><span data-stu-id="4d4ea-652">json_array:key</span></span>          | <span data-ttu-id="4d4ea-653">valueA</span><span class="sxs-lookup"><span data-stu-id="4d4ea-653">valueA</span></span> |
| <span data-ttu-id="4d4ea-654">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="4d4ea-654">json_array:subsection:0</span></span> | <span data-ttu-id="4d4ea-655">valueB</span><span class="sxs-lookup"><span data-stu-id="4d4ea-655">valueB</span></span> |
| <span data-ttu-id="4d4ea-656">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="4d4ea-656">json_array:subsection:1</span></span> | <span data-ttu-id="4d4ea-657">valueC</span><span class="sxs-lookup"><span data-stu-id="4d4ea-657">valueC</span></span> |
| <span data-ttu-id="4d4ea-658">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="4d4ea-658">json_array:subsection:2</span></span> | <span data-ttu-id="4d4ea-659">valueD</span><span class="sxs-lookup"><span data-stu-id="4d4ea-659">valueD</span></span> |

<span data-ttu-id="4d4ea-660">Nell'app di esempio, la classe POCO seguente è disponibile per associare le coppie chiave-valore della configurazione:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-660">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="4d4ea-661">Dopo l'associazione, `JsonArrayExample.Key` contiene il valore `valueA`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-661">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="4d4ea-662">I valori di sottosezione vengono archiviati nella proprietà matrice POCO `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-662">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="4d4ea-663">Indice di `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="4d4ea-663">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="4d4ea-664">Valore di `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="4d4ea-664">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="4d4ea-665">0</span><span class="sxs-lookup"><span data-stu-id="4d4ea-665">0</span></span>                                   | <span data-ttu-id="4d4ea-666">valueB</span><span class="sxs-lookup"><span data-stu-id="4d4ea-666">valueB</span></span>                              |
| <span data-ttu-id="4d4ea-667">1</span><span class="sxs-lookup"><span data-stu-id="4d4ea-667">1</span></span>                                   | <span data-ttu-id="4d4ea-668">valueC</span><span class="sxs-lookup"><span data-stu-id="4d4ea-668">valueC</span></span>                              |
| <span data-ttu-id="4d4ea-669">2</span><span class="sxs-lookup"><span data-stu-id="4d4ea-669">2</span></span>                                   | <span data-ttu-id="4d4ea-670">valueD</span><span class="sxs-lookup"><span data-stu-id="4d4ea-670">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="4d4ea-671">Provider di configurazione personalizzato</span><span class="sxs-lookup"><span data-stu-id="4d4ea-671">Custom configuration provider</span></span>

<span data-ttu-id="4d4ea-672">L'app di esempio dimostra come creare un provider di configurazione di base che legge le coppie chiave-valore di configurazione da un database usando [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-672">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="4d4ea-673">Il provider ha le caratteristiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-673">The provider has the following characteristics:</span></span>

* <span data-ttu-id="4d4ea-674">Il database in memoria di Entity Framework viene usato a scopo dimostrativo.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-674">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="4d4ea-675">Per usare un database che richiede una stringa di connessione, implementare un `ConfigurationBuilder` secondario per fornire la stringa di connessione da un altro provider di configurazione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-675">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="4d4ea-676">Il provider legge una tabella di database in una configurazione all'avvio.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-676">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="4d4ea-677">Il provider non esegue una query sul database per ogni chiave.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-677">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="4d4ea-678">Il ricaricamento in caso di modifica non è implementato, quindi l'aggiornamento del database dopo l'avvio dell'app non ha alcun effetto sulla configurazione dell'app.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-678">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="4d4ea-679">Definire un'entità `EFConfigurationValue` per l'archiviazione dei valori di configurazione nel database.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-679">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="4d4ea-680">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-680">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="4d4ea-681">Aggiungere `EFConfigurationContext` per archiviare i valori configurati e accedervi.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-681">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="4d4ea-682">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-682">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="4d4ea-683">Creare una classe che implementi <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-683">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="4d4ea-684">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-684">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="4d4ea-685">Creare il provider di configurazione personalizzato ereditando da <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-685">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="4d4ea-686">Il provider di configurazione inizializza il database quando è vuoto.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-686">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="4d4ea-687">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-687">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="4d4ea-688">Un metodo di estensione `AddEFConfiguration` consente di aggiungere l'origine di configurazione a un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-688">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="4d4ea-689">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-689">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="4d4ea-690">L'esempio di codice seguente mostra come usare il `EFConfigurationProvider` personalizzato in *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-690">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="4d4ea-691">Accedere alla configurazione durante l'avvio</span><span class="sxs-lookup"><span data-stu-id="4d4ea-691">Access configuration during startup</span></span>

<span data-ttu-id="4d4ea-692">Inserire `IConfiguration` nel costruttore `Startup` per accedere ai valori di configurazione in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-692">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="4d4ea-693">Per accedere alla configurazione in `Startup.Configure`, inserire `IConfiguration` direttamente nel metodo o usare l'istanza dal costruttore:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-693">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="4d4ea-694">Per un esempio di accesso alla configurazione usando metodi di servizio di avvio, vedere [Avvio dell'applicazione: Metodi pratici](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="4d4ea-694">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="4d4ea-695">Accedere alla configurazione in una pagina Razor Pages o in una visualizzazione MVC</span><span class="sxs-lookup"><span data-stu-id="4d4ea-695">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="4d4ea-696">Per accedere alle impostazioni di configurazione in una pagina Razor Pages o in una visualizzazione MVC, aggiungere un [direttiva using](xref:mvc/views/razor#using) ([Informazioni di riferimento su C#: direttiva using](/dotnet/csharp/language-reference/keywords/using-directive)) per lo [spazio dei nomi Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) e inserire <xref:Microsoft.Extensions.Configuration.IConfiguration> nella pagina o nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-696">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="4d4ea-697">In una pagina Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-697">In a Razor Pages page:</span></span>

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

<span data-ttu-id="4d4ea-698">In una visualizzazione MVC:</span><span class="sxs-lookup"><span data-stu-id="4d4ea-698">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="4d4ea-699">Aggiungere la configurazione da un assembly esterno</span><span class="sxs-lookup"><span data-stu-id="4d4ea-699">Add configuration from an external assembly</span></span>

<span data-ttu-id="4d4ea-700">Un'implementazione <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> consente l'aggiunta di miglioramenti a un'app all'avvio, da un assembly esterno alla classe `Startup` dell'app.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-700">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="4d4ea-701">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="4d4ea-701">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4d4ea-702">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4d4ea-702">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* <span data-ttu-id="4d4ea-703">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/) (Configurazione Microsoft in dettaglio)</span><span class="sxs-lookup"><span data-stu-id="4d4ea-703">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)</span></span>
